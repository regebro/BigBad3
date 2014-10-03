:skip-help: true
:title: Who's Afraid of the Big Bad 3
:auto-console: true

----

Who's Afraid of the Big Bad 3
=============================

Lennart Regebro
---------------

PyCon Ireland, Dublin 2014

----

Python 3 is really a new language!
==================================

----

Everything breaks!
==================

----

No other language ever break backwards compatibility!
=====================================================

----

Python 3 was a mistake!
=======================

----

There are no third-party modules!
=================================

----

Who wants Python 3 anyway?
==========================

Python 2 is good enough!
------------------------

----

You will have to spend hundreds of man-hours!
=============================================

----

Or?
===

----

Python 3 is Python!
===================

.. note::

    If it was a new language, then you would get confused when reading Python 3 code.
    You would not be entirely sure what the code would do, as bits of it wouldn't make sense.
    That is not the case, I promise.

----

Does everything break?
======================

.. note::

    Not all code breaks, but yes, every non-trivial package is likely to break.
    But that does not mean it's hard to fix, and I'll look at that later.

-----

Does other languages break backwards compatibility?
===================================================

.. note::

    In August I saw Armin Ronacher tweet about compiling early versions of Python.
    It took a bit of effort and even then Python apparently crashed on exit.
    Is C then really backwards compatible?
    What does backwards compatible mean?

    Well it doesn't mean that code will continue to run forever.
    What it really means is that when it breaks, we can fix it so that it runs on both the old and the new version.
    And with Python 3 we need hacks and compatibility layers for that, so we say it's not backwards compatible.
    But that was for comparing Python 3.0 to Python 2 in general.
    With Python 3.3 and Python 2.7, it's not so clear.
    You can write code that runs on both quite easily.

----

Was Python 3 a mistake?
=======================

.. note::

    There was several mistakes that could only be fixed by breaking backwards compatibility.

    * Floor division

    * Exception syntax

    * Comparing strings and numbers.

    * Unicode

    C++ has 84 keywords, Python has 33.
    This is a big reason that Python is popular: Python fits your brain.
    And if we want Python to continue to be everyones favourite language, that simplicity must remain.

    C++ 11 had 10 new keywords.
    Python 3 had only one really new keyword, nonlocal.
    False, True and None was made keywords as well, but they existed before, so they don't count.
    It also had two keywords taken away (exec and print).

    So I don't think Python 3 was a mistake.
    I think it's necessary to keep is small and understandable.

----

There are Third-party modules!
==============================

* 165 of the 200 top packages on the Cheeseshop support Python 3

* Over 4000 Python 3 packages on the Cheeseshop.

.. image:: images/py3pkgs.png

.. note::

    165 of 200 are not too shabby.
    And 3 packages (Paste, python-cloudfiles, ssh) is deprecated and will not be ported.
    6 packages (supervisor, fabric, Deliverance, sentry, tiddlywebplugins.tiddlyspace, flexget) is not libraries,
    but applications so you don't really need Python 3 support very much.

    So really, it's only 26 of the top 200 packages that still need to support Python 3.
    And work is ongoing for most of them.

----

You want Python 3
=================

Although you might not know it yet
----------------------------------

(Bits of this shamelessly ripped from Aaron Meurer's talk)

----

Extended Iterable Unpacking
===========================

.. code::

    >>> first, second, *rest, last = "a b c d e f".split()
    >>> first, second, last
    ('a', 'b', 'f')

.. note::

    The `*rest` bit will suck up anything that doesn't end up in any other variables.
    You can only have one `*rest` per line, of course, but you can have both a first and a second, etc.

----

Keyword only arguments
======================

.. code::

    >>> def foo(a, *args, b, **kw):
    ...   print(a, b, args, kw)

    >>> foo(1, 2, 3, b=4, c=5)
    1 4 (2, 3) {'c': 5}

.. note::

    This looks like the Extended Iterable Unpacking!
    The main effect of that is that you HAVE to pass in b as a keyword paremeter.

----

Chained exceptions
==================

.. code::

    >>> try:
    ...     1/0
    ... except Exception as e:
    ...     raise KeyError("wut?") from e
    Traceback (most recent call last):
      File "<stdin>", line 2, in <module>
    ZeroDivisionError: division by zero

    The above exception was the direct cause of the following exception:

    Traceback (most recent call last):
      File "<stdin>", line 4, in <module>
    KeyError: 'wut?'

.. note::

    In Python 2, if you raise an exception during exception handling, the original exception is lost.
    In Python 3 you can chain them, and get both tracebacks, which is really handy for debugging.
    You don't actually have to explicitly chain them in this case, they will be implicitly chained.
    But raise from will chain exceptions even when it's not in a try/except case.

----

Better OS Exceptions
====================

.. note::

    Is Python 2, loads of errors are hidden behind the OSError exceptions.
    In Python 3,3, you have many separate exceptions, which all inherit from OSerror.
    For example you can now get a FileExistsError and a NotADirectoryError.
    This makes it much simpler to handle different errors separately.
    Also other operating system errors like IOError, are also now subclasses of OSError.

----

File handle warnings
====================

.. note::

    If you don't close a file, you will get a warning when the file object is garbage collected.
    Very nice to make sure you don't leave open files around.

----

Yield from
==========

.. note::

    You also have `yield from`, which let's you delegate your generator to a subgenerator.
    Extremely handy.

----

Simply super
============

Python 2
--------

.. code::

    super(ClassName, self).method(foo, bar)


Python 3
--------

.. code::

    super().method(foo, bar)

----

asyncio
=======

.. note::

    There are several new modules in later versions of Python 3.
    Most of them have backports so you can use them anyway.
    I think enum perhaps is the most interesting one there.

    But one does not have a Python 2 backport, and that's asyncio, previously known as Tulip.
    It seems very cool, and you need Python 3.3 or later for that.

----

Hundreds of man-hours? Really?
==============================

.. note::

    Well, this really depends on the code you need to fix, and how much code of course.
    But I have added Python 3 support to a whole bunch of libraries, and perhaps I have spent hundreds of hours on this.

    Well, no, not perhaps, I have spent hundreds of man hours on it.
    But these were some really hard libraries to move to Python 3, and I ported them to Python 3.0 or 3.1,
    which are much harder to port to than Python 3.3 and later.
    I also needed them to run on Python 2.5 or even Python 2.4, adding a whole extra player of problems.

    So this might have been True in 2008 or 2009, both because you needed to support Python 2.4 and Python 3.1,
    but also because less libraries were available,
    so you needed to port more libraries that you didn't write.

    But today the situation is very different.
    I'm going to talk about this later, with a real world example.

----

Supporting Python 3 is not so bad
=================================

.. note::

    Although every package is likely to break in some way, most code will not break.

----

Many changes are handled by 2to3
================================

* Exception syntax

* `print` is a function

* `xrange` is gone

* Standard library reorganisation

* etc...

.. note::

    Most changes are handled by 2to3, but maybe not always in the prettiest way.

----

Some changes need no handling at all
====================================

* dict.keys() no longer returns a list

* Indentation is stricter

* Long and Int are merged

.. note::

    Other changes typically will not affect you at all, unless you are violating good coding practices.

----

If you need Python 2 compatibility
==================================

.. code:: python

    from __future__ import division
    from __future__ import print__function

    print("Three halves is written", 3/2, "with decimals.")

.. note::

    Other changes has explicit forward compatibility, like the new division and the print function.
    This is useful if you need to keep Python 2 compatibility,
    which you typically only need if you are adding Python 3 support to a library.

----

u'' is back!
============

.. note::

    Some backwards compatibility has also been added back in later Python 3 versions.
    The most important of those is that in Python 3.3 the u'' prefix for Unicode was added back.
    In addition there are now libraries out there that will help you, like six and futurize.

    This means that as long as you don't need to support Python 2.5 or Python 3.2,
    writing code that runs on both Python 2 and Python 3 is not that hard.

    So what IS hard?

----

API changes
===========

.. note::

    If you need to change your libraries API to be Python 3 compatible, that's a pain.

----

Example 1: zope.component
=========================

.. code::

    class TheComponent(object):
        implements(ITheInterface)


.. note::

    This syntax used in Python 2 relies on how metaclasses work in Python 2.
    The implements statement is actually executed, and it inserts a metaclass in the local context,
    which in turn makes the class creation use a metaclass.

    This doesn't work in Python 3, because metaclasses are not declared in the class body.

----


Example 1: zope.component
=========================

.. code::

    @implementor(ITheInterface)
    class TheComponent(object):
        pass

.. note::

    But instead there is now class decorators.
    So the API needed to change.

    Lesson learned: Don't use Python magic as an API.
    That said, when this API was created in 2001 there wan't much choice.

    A fixer was needed to make it possible to change the API with 2to3.
    Writing fixers is HARD partly because it's badly documented.
    Try to avoid it.

----

Example 2: icalendar
====================

.. code::

    ical = str(icalendarobject)

.. note::

    In the module called icalendar there are icalendar objects.
    These represent an icalendar file, and to make the file you just make it into a string.
    The result is a UTF-8 encoded iCalendar string.

    But in Python 3, strings are Unicode. So this fails.

----

Example 2: icalendar
====================

.. code::

    ical = icalendarobject.to_ical()

.. note::

    Much better.

    Lesson learned: Don't use dunder methods as an API.

----

Bytes/Strings/Unicode
=====================

.. note::

    And you may then wonder what it is that prompts some influential heavyweights to complain so much about Python 3.
    And the biggest issue is bytes/strings/unicode.

    But avoiding strings, bytes and Unicode is less easy.
    And the biggest issue is that the API for bytes and strings are slightly different.
    For example, if you iterate over a string, the values you get are one-character strings.
    However, if you iterate over a bytes string, you get integers!
    There are other differences as well, and this makes it hard to support both bytes and strings with the same API,
    which is something you often want to do.
    You get similar problems with supporting both strings and Unicode under Python 2.
    For example, the new io.StringIO class will only work with Unicode.

----

You gotta keep'em separated!
============================

.. note::

    This means that you need to always cleanly separate when you work with binary data,
    and when you work with textual data.
    Don't use the same variables for both Unicode text and binary data, if you can avoid it.

    In Python 2 you often did not need to make such a separation.
    That led to a lot of confusion with regards to Unicode, and a lot of problems.

----

Practical Experiences
=====================

.. note::

    When preparing for this talk I decided to look at the current state of Python 3 support.
    I wanted to know how difficult it would typically be to help port the libraries you depend on.
    To do that I needed to port some package that I didn't already know intimately, and decided on Diazo.

    I picked Diazo because I looked at the Python Wall of Superpowers. https://python3wos.appspot.com/
    Most of the modules support Python 3 already.
    And those who do not often already have Python 3 support efforts.

    But far down I found "Deliverence".
    Deliverence doens't have Python 3 support and there are two reasons for that.
    One is that it's a standalone program, and not a library, so it not supporting Python 3 is not a big problem.
    The other is that although less popular, Diazo is generally a better alternative, which is why I decided to port Diazo.

    Let me first explain what Deliverence and Diazo does.

----

Deliverance
===========

.. note::

    Deliverence and Diazo takes two HTML pages and maps bits of one page into another page according to a rule-set.
    It means you can have a designer create the design as static HTML and then you can map your dynamic site into that design without even modifying your site.
    So you can style your PHP site or your Plone site without actually knowing either PHP or Plone.
    Brilliant! We've used it on pretty much any site I've been involved with the last 4 years.


----

Diazo
=====

.. note::

    Diazo takes the same concepts and the same rule syntax as Deliverence, but it actually compiles the rules into XSLT.
    You can then let nginx or apache do this mapping.
    Or you can use the included WSGI server, or you can use it as a library inside your web framework.

    So, how did I add Python 3 support?

----

Tool 1: caniusepython3
======================

.. note::

    This is both a command line tool and a website. https://caniusepython3.com/
    It's not perfect, but it's helpful as a way to evaluate the application.
    It told me it needed repoze.xmliter, but I think they have changed how it works slightly.
    Now it only reports the packages that do not support Python 3, but where all dependencies support Python 3.
    So in other words, caniusepython3 will now essentially recommend which package I should add Python 3 support to first.

    In any case I started porting repoze.xmliter, and it will during testing use another module, collective.checkdocs that didn't support Python 3.

----

Adding Python 3 support to collective.checkdocs
===============================================

The collective.checkdocs source is on the Plone Collective svn server,
which is in read only mode, so I need to first migrate it to the collective repo on github.
I started that process (svn2git takes hours to run on that repository, it's huge)
and I mailed the original author to make sure that he is OK with it.

Once I got the OK from the original author I then added some simple tests to the module as it had no tests.

----

Tool 2: Virtualenv
==================

Tox can help you run tests on a module for several Python versions.
It's a big buyers beware here, though.
Tox used to be good, but now it is starting to be quite brittle, and doesn't work with all Python versions etc.
In my experience the last months, it's now more trouble that it's worth.
I have unfortunately not had any time to actually dig into the problems, with Tox.
Hopefully this will get better in the future.

But I have instead of using Tox simply set up virtualenvs for each version I want to test:

    /opt/python26/bin/virtualenv .py26
    ./.py26/bin/python setup.py develop

And then I simply run the tests with

    ./.py26/bin/python setup.py test

etc. It's a little bit more work to get started, but unlike using Tox is actually worked.

----

Tool 3: 2to3
============

I then ran 2to3 on the code to update things to Python 3.
It doesn't work perfectly, I need to clean up the imports manually.
I also need to add a from __future__ import print_function to get it to run under Python 2.

I add Python 3.2, 3.3 and 3.4 to the list of supported versions in setup.py.
I clean up things a bit, add a MANIFEST.in etc, makes sure Pyroma thinks the code is creamy, and release the module to PyPI.

Total time spent, including setting up Tox and then not using it anyway: Around 4 hours. The new version is released already.

----

Adding Python 3 support to repoze.xmliter
=========================================

repoze.xmliter is a wrapper to lxml that you can iterate over.
It will then give you chunks of byte strings of XML.
Not the most exiting module on PyPI, but interesting for this project, because it needs to handle both binary data and text!
This as we know, make it a Tricky Module to support Python 3.

----

Tool 4: Futurize
================

Futurize is an extension to Python 3 that supposedly keep Python 2 compatibility when doing the fixes.

So I tried to use futurize here, but that doesn't work.
After running futurize the code stopped working in Python 2, and still did not work in Python 3.
I fought with this a bit, and ended up starting over.

What I end up doing is running the tests under Python 3, and fixing error by error,
while after each fix running it under Python 2 to make sure it didn't break.

And here we come to one of the biggest complaints about Python 3 that is actually true.
This type of code often ends up ugly.
There are a lot of checks for if the input is byte strings or Unicode strings, and as we all know, type checking is unpythonic.

In this case I could cheat, because the relevant methods take an encoding parameter,
so now you can either pass in what encoding the byte string is using,
or you pass in the unicode object instead of a name of an encoding.
So I don't actually do type checking, it's inferred otherwise in this case.
But often you need to check the type.

I also needed to add tests to make sure Unicode was supported.
It was, but there were no tests for it.

In total the work to port, including false starts, cleanups and added tests was no more than 6 hours.

----

Adding Python 3 support to Diazo
================================

Now time had come to Diazo itself.
With Diazo I again first quickly tried to run the code through futurize to see if it would still work with Python 2 afterwards.
Again it would not, so I did the same thing I did with repoze.xmliter, and would run the tests under Python 3,
fix a test failure, make sure it still ran under Python 2, and then repeat.

In the case of Diazo I was affected a lot by the import reorganization, so what I did here was that I included future as a dependency,
and I when I found a problem that could be solved by a fixer, I ran that specific fixer on the code with

futurize -w -f <thespecificfixer> .

The main thing I needed to do manually after this was change all the tests to use byte strings instead of native strings,
and switch from cStringIO to io.BytesIO.

Total time: 3 hours

----

Updating the documentation
==========================

The Diazo buildout includes a default test setup with Paste so you can develop your theme rules without nginx or Apache.
But Paste is not and will not be ported to Python 3.
The test setup also uses a lot of Paste apps, like urlmap and proxy, so I can't just switch it out for any old  WSGI server.
I needed one that used PasteDeploy.
A Python 3 compatible server designed to replace Paste's server exists in gearbox, but what about the apps?

----

Tool 5: Twitter!
================

I was discussing the issue on Twitter as I was preparing to port Paste's static and proxy apps.
The urlmap app was already ported as "rutter".
But then Ian Bicking pointed out that the apps I wanted to port already had been ported and was a part of WebOb!
However, it did not have any PasteDeploy entry points, so I needed to fix that.

----

webobentrypoints
================

So I started a package simply called "webobentrypoints".
As of today, it only contains PasteDeploy entry points for the static directory app and using the client app as a proxy,
because that's what was needed. I'll try to get time to add entry points for the other apps as well.

This took a long time because I neede to learn about the PasteDeploy entry points,
and I needed to re-learn WSGI which I hadn't looked at for years.
All in all this probably took 4-6 hours, of which maybe one was spent actually making the webobentrypoints package.

----

Less than 20 hours!
===================

That included porting collective.checkdocs, repoze.xmliter, Diazo and writing webobentrypoints.
Much of the time was not spent actually porting, but learning what the various modules actually did.


----

Conclusions
===========
