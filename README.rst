``ipaddress`` backport for Python 2.6 and 2.7
=============================================

This is a Python 2.6 backport of the Python 3.4 ``ipaddress`` module.

Please refer to the `official Python 3.4 documentation`__ for more information
on the module.

__ http://docs.python.org/3.4/library/ipaddress


Differences from Python 3.4's ``ipaddress`` module
--------------------------------------------------

The backport should behave identically to 3.4, except as noted here.


``bytearray`` instead of ``bytes``
..................................

Since Python 2 has no distinct ``bytes`` type, ``bytearray`` is used
instead for the "packed" (binary) address representation::

    >>> ipaddress.ip_address(bytearray('\xc0\xa8\x54\x17'))
    IPv4Address('192.168.84.23')
    >>> ipaddress.ip_address('127.0.0.17').packed
    bytearray(b'\x7f\x00\x00\x11')

This means py2-ipaddress can accept both Python 2 string types (``str``
and ``unicode``) for the textual address representation.

If you prefer semantics closer to Python 3, you may be interested in
Philipp Hagemeister's `ipaddress backport`__, which uses ``str`` for
the "packed" address representation, but then requires all textual IP
addresses to be given as ``unicode`` strings. That backport also
supports Python 3.0–3.2.

__ https://github.com/phihag/ipaddress/


Caching of ``is_private`` and ``is_global``
...........................................

Since Python 2.7's ``functools`` module does not have Python 3.2's
``lru_cache``, no caching is performed for the ``is_private`` and
``is_global`` properties; this should be a minor problem as Python 3.3's
``ipaddress`` did not use ``lru_cache`` either.


Changelog
---------

py2-ipaddress 3.4.2
...................

Test suite and documentation improvements.


py2-ipaddress 3.4.1
...................

Python 2.6 support and a bugfix.


py2-ipaddress 3.4
.................

Since Python 2 does not distinguish between ``bytes`` and ``str`` like
Python 3 does, version 2.0.1 and earlier of py2-ipaddress attempted to
interpret ``str`` arguments as *both* and do the "right" thing.

This unfortunately led to surprising behavior in py2-ipaddress::

    >>> ipaddress.ip_address('test.example.org')
    IPv6Address('7465:7374:2e65:7861:6d70:6c65:2e6f:7267')

The ``ipaddress`` module does not, of course, perform DNS resolution.
Rather, the argument is interpreted as a byte string (of length 16) and
converted bit-for-bit into an IPv6 address. In Python 3, ``ipaddress``
correctly rejects such a constructor argument (unless the ``b`` prefix
is used to explicitly mark the literal as a byte string).

Even worse, there is not always a single right interpretation. Python 3
example::

    >>> ipaddress.ip_address('::1234:5678:9abc')
    IPv6Address('::1234:5678:9abc')
    >>> ipaddress.ip_address(b'::1234:5678:9abc')
    IPv6Address('3a3a:3132:3334:3a35:3637:383a:3961:6263')

There is no way to distinguish the two invocations in Python 2. As a
result, py2-ipaddress 3.4 uses ``bytearray`` for all byte strings, and
``str`` for text strings only::

    >>> ipaddress.ip_address('::1234:5678:9abc')
    IPv6Address('::1234:5678:9abc')
    >>> ipaddress.ip_address(b'::1234:5678:9abc')
    IPv6Address('::1234:5678:9abc')
    >>> ipaddress.ip_address(bytearray('::1234:5678:9abc'))
    IPv6Address('3a3a:3132:3334:3a35:3637:383a:3961:6263')


License
-------

The ``ipaddress`` modules (both the original and this backport) are licensed
under the `Python Software Foundation License version 2`__.

The modifications made for Python 2.6 compatibility are hereby released into
the public domain by the authors.

__ https://www.python.org/download/releases/3.4.0/license
