python-zeroconf
===============

.. image:: https://travis-ci.org/jstasiak/python-zeroconf.svg?branch=master
    :target: https://travis-ci.org/jstasiak/python-zeroconf
    
.. image:: https://img.shields.io/pypi/v/zeroconf.svg
    :target: https://pypi.python.org/pypi/zeroconf

.. image:: https://img.shields.io/coveralls/jstasiak/python-zeroconf.svg
    :target: https://coveralls.io/r/jstasiak/python-zeroconf

    
This is fork of pyzeroconf, Multicast DNS Service Discovery for Python,
originally by Paul Scott-Murphy (https://github.com/paulsm/pyzeroconf),
modified by William McBrine (https://github.com/wmcbrine/pyzeroconf).

This fork is used in all of my TiVo-related projects: HME for Python
(and therefore HME/VLC), Network Remote, Remote Proxy, and pyTivo.
Before this, I was tracking the changes for zeroconf.py in three
separate repos. I figured I should have an authoritative source.

Although I make changes based on my experience with TiVos, I expect that
they're generally applicable. This version also includes patches found
on the now-defunct (?) Launchpad repo of pyzeroconf, and elsewhere
around the net -- not always well-documented, sorry.

Compatible with:

* Bonjour
* Avahi

Compared to some other Zeroconf/Bonjour/Avahi Python packages, python-zeroconf:

* isn't tied to Bonjour or Avahi
* doesn't use D-Bus
* doesn't force you to use particular event loop or Twisted
* is pip-installable
* has PyPI distribution

Python compatibility
--------------------

* CPython 2.6, 2.7, 3.3+
* PyPy 2.2+ (possibly 1.9-2.1 as well)
* PyPy3 2.4+

Versioning
----------

This project's versions follow the following pattern: MAJOR.MINOR.PATCH.

* MAJOR version has been 0 so far
* MINOR version is incremented on backward incompatible changes
* PATCH version is incremented on backward compatible changes


How to get python-zeroconf?
===========================

* PyPI page https://pypi.python.org/pypi/zeroconf
* GitHub project https://github.com/jstasiak/python-zeroconf

The easiest way to install python-zeroconf is using pip::

    pip install zeroconf



How do I use it?
================

Here's an example:

.. code-block:: python

    from six.moves import input
    from zeroconf import ServiceBrowser, Zeroconf
    
    
    class MyListener(object):
    
        def remove_service(self, zeroconf, type, name):
            print("Service %s removed" % (name,))
    
        def add_service(self, zeroconf, type, name):
            info = zeroconf.get_service_info(type, name)
            print("Service %s added, service info: %s" % (name, info))
    
    
    zeroconf = Zeroconf()
    listener = MyListener()
    browser = ServiceBrowser(zeroconf, "_http._tcp.local.", listener)
    try:
        input("Press enter to exit...\n\n")
    finally:
        zeroconf.close()

.. note::

    Discovery and service registration use *all* available network interfaces by default.
    If you want to customize that you need to specify ``interfaces`` argument when
    constructing ``Zeroconf`` object (see the code for details).

See examples directory for more.

Changelog
=========

0.17.2
------

* Fixed installation on Python 3.4.3+ (was failing because of enum34 dependency
  which fails to install on 3.4.3+, changed to depend on enum-compat instead;
  thanks to Michael Brennan for the original patch, GitHub pull request #22)

0.17.1
------

* Fixed EADDRNOTAVAIL when attempting to use dummy network interfaces on Windows,
  thanks to daid

0.17.0
------

* Added some Python dependencies so it's not zero-dependencies anymore
* Improved exception handling (it'll be quieter now)
* Messages are listened to and sent using all available network interfaces
  by default (configurable); thanks to Marcus Müller
* Started using logging more freely
* Fixed a bug with binary strings as property values being converted to False
  (https://github.com/jstasiak/python-zeroconf/pull/10); thanks to Dr. Seuss
* Added new ``ServiceBrowser`` event handler interface (see the examples)
* PyPy3 now officially supported
* Fixed ServiceInfo repr on Python 3, thanks to Yordan Miladinov

0.16.0
------

* Set up Python logging and started using it
* Cleaned up code style (includes migrating from camel case to snake case)

0.15.1
------

* Fixed handling closed socket (GitHub #4)

0.15
----

* Forked by Jakub Stasiak
* Made Python 3 compatible
* Added setup script, made installable by pip and uploaded to PyPI
* Set up Travis build
* Reformatted the code and moved files around
* Stopped catching BaseException in several places, that could hide errors
* Marked threads as daemonic, they won't keep application alive now

0.14
----

* Fix for SOL_IP undefined on some systems - thanks Mike Erdely.
* Cleaned up examples.
* Lowercased module name.

0.13
----

* Various minor changes; see git for details.
* No longer compatible with Python 2.2. Only tested with 2.5-2.7.
* Fork by William McBrine.

0.12
----

* allow selection of binding interface
* typo fix - Thanks A. M. Kuchlingi
* removed all use of word 'Rendezvous' - this is an API change

0.11
----

* correction to comments for addListener method
* support for new record types seen from OS X
  - IPv6 address
  - hostinfo

* ignore unknown DNS record types
* fixes to name decoding
* works alongside other processes using port 5353 (e.g. on Mac OS X)
* tested against Mac OS X 10.3.2's mDNSResponder
* corrections to removal of list entries for service browser

0.10
----

* Jonathon Paisley contributed these corrections:
  - always multicast replies, even when query is unicast
  - correct a pointer encoding problem
  - can now write records in any order
  - traceback shown on failure
  - better TXT record parsing
  - server is now separate from name
  - can cancel a service browser
* modified some unit tests to accommodate these changes

0.09
----

* remove all records on service unregistration
* fix DOS security problem with readName

0.08
----

* changed licensing to LGPL

0.07
----

* faster shutdown on engine
* pointer encoding of outgoing names
* ServiceBrowser now works
* new unit tests

0.06
----
* small improvements with unit tests
* added defined exception types
* new style objects
* fixed hostname/interface problem
* fixed socket timeout problem
* fixed add_service_listener() typo bug
* using select() for socket reads
* tested on Debian unstable with Python 2.2.2

0.05
----

* ensure case insensitivty on domain names
* support for unicast DNS queries

0.04
----

* added some unit tests
* added __ne__ adjuncts where required
* ensure names end in '.local.'
* timeout on receiving socket for clean shutdown


License
=======

LGPL, see COPYING file for details.
