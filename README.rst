``ipaddress`` backport for Python 2.7
=====================================

This is a Python 2.7 backport of the Python 3.3 ``ipaddress`` module.

Not all 3.3 functionality is supported, due to the lack of a distinct ``bytes``
type in Python 2.7. Nevertheless, it is quite useful if you're stuck with
Python 2.7.

Please refer to the `official Python 3.3 documentation`__ for more information
on the module.

__ http://docs.python.org/3.3/library/ipaddress


License
-------

The ``ipaddress`` modules (both the original and this backport) are licensed
under the `Python Software Foundation License version 2`__.

The modifications made for Python 2.7 compatibility are hereby released into
the public domain by the authors.

__ https://www.python.org/download/releases/3.3.0/license