:skip-help: true
:title: Prehistoric Patterns in Python
:auto-console: false

----

Who's Afraid of the Big Bad 3
=============================

Lennart Regebro
---------------

PyCon Ireland, Dublin 2014

----

Python 3 is a dead end!
=======================

----

Everything breaks!
==================

----

There are no third-party modules!
=================================

----

You will have to spend hundreds of man-hours!
=============================================

----

Python 3 is really a new language!
==================================

----

No other language ever break backwards compatibility!
=====================================================

----

Python 3 was a mistake!
=======================

----

Python 2 is good enough!
========================

----

Or?
===

----

Is Python 3 a dead end?
=======================

.. note::

    Sometimes people claim that Python 3 will fail, and that it is a dead end. But...

----

All new features go into Python 3!
==================================


.. note::

    ...all new features go into Python 3!

    Some of the new features are available as libraries, but not all.

    Most people that ask for a Python 2.8 want all new Python 3 features to be backmerged to Python 2.
    Doesn't sound like Python 3 is dead to me...

----

Reality: Python 2 is the dead end
=================================

.. note::

    So the reality is that it's Python 2 that is the dead end.
    This even sometimes is brought forward as an argument against Python 3, strangely enough.
    All new releases make the old releases dead-ends, that's the whole point of making new releases...

----

Does everything break?
======================

.. note::

    It's hard to argue with this, because although not everything breaks,
    you do in general have to make changes to your code.
    I've so far only found one package that actually ran on Python 3 without modifications, and it was very simple.
    And even then, the tests broke and I had to fix them.

    So yes, every package is likely to break. But that doens't mean it's hard to fix it.

----

There are Third-party modules!
==============================

* 165 of the 200 top packages support Python 3

* Over 4000 Python 3 packages on the Cheeseshop.

.. image:: images/py3pkgs.png

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

Python 3 is familiar!
=====================

.. note::

    If it was a new language, then you would get confused when reading Python 3 code.
    You would not be entirely sure what the code would do, as bits of it wouldn't make sense.
    That is not the case, I promise.
    Python 3 code looks exactly like Python 2 code, with some minor differences.
    The most telltale sign is usually that print is a function now.
    If anything, Python 3 code is clearer, as 3/2 returns one and a half now.

----

Python 3 was a mistake
======================

.. note::

    There was several mistakes that could only be fixed by breaking backwards compatibility.

    * Floor division

    * Exception syntax

    * Unicode

    So I don't think Python 3 was a mistake.

----

Backwards compatibility
=======================

.. note::

    Loads of other languages try to not break backwards compatibility, they just load more and more stuff on,
    making the language more and more complex.
    Is that really what we would like to do to Python?
    C++ has 84 keywords, 10 of them was new in C++ 11.
    Python has 33, 4 of them is new in Python 3 (False, True, None and nonlocal) and two taken away (exec and print).
    Recently there has been reports that Python is now the number one language used in beginners programming classes on universities.
    This is the reason for that.
    And if we want Python to continue to be everyones favourite language, that simplicity must remain.

----

Is a fuzzy concept
==================

.. note:

    I saw Armin Ronacher abtrying to compile  early versions of Python.


----

Python 2 is good enough
=======================

.. note::

    Well, I can't argue with that one.
    I agree, it is good enough.
    But Python 3 *is* better.

----

Most changes are not so bad
===========================

.. note::

   uh,

----

.. code:: python

    except Exception, e:

Turned into

.. code:: python

    except Exception as e:

.. note::

    The first syntax is not allowed in Python 3.
    But, the second syntax is allowed in Python 2.6 and 2.7.
    That means that you can perfectly well write code that runs on both Python 2 and Python 3 using the new syntax,
    as long as you don't need to support versions before Python 2.6.

----

Other changes has explicit forward compatibility, like the new division and the print function:

.. code:: python

    from __future__ import division
    from __future__ import print__function

    print("Three halves is written", 3/2, "with decimals.")


Some backwards compatibility has also been added back in Python 3.
The most important of those is that in Python 3.3 the u'' prefix for Unicode was added back.
In addition there are now libraries out there that will help you, like six and futurize.

This means that as long as you don't need to support Python 2.5 or Python 3.2,
writing code that runs on both Python 2 and Python 3 is not that hard.


But when it's bad, it's really bad
==================================

And you may then wonder what it is that prompts some influential heavyweights to complain so much about Python 3.
And the biggest issue is bytes/strings/unicode.

Unless you use doctests, then doctests is the biggest issue.
If you are using doctests, don't use doctests.

But avoiding string, bytes and Unicode is less easy.
And the biggest issue is that the API for bytes and strings are slightly different.
For example, if you iterate over a string, the values you get are one-character strings.
However, if you iterate over a bytes string, you get integers!
There are other differences as well,
and this makes it hard to support both bytes and strings with the same API,
which is something you often want to do.
You get similar problems with supporting both strings and Unicode under Python 2.
For example, the new io.StringIO class will only work with Unicode.

This means that you need to always cleanly separate when you work with binary data,
and when you work with textual data.
In Python 2 you often did not need to make such a separation.
That led to a lot of confusion with regards to Unicode, and a lot of problems,
but if your code is working, this new setup means more work for you.

Other cases when it's not fun is when your API don't work under Python 3, or won't make sense.
The icalendar module had an API where you used str(icalendar) to generate the UTF-8 encoded icalendar output.
Obviously that doesn't work in Python 3, as str(icalendar) would generate Unicode, not an 8-bit string.
The API needed to be changed.
lxml has a .tostring() method, which will give you bytes under Python 3, unless you explicitly pass in the encoding 'unicode'.
This can be confusing...


Practical Experiences
=====================

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
Deliverence and Diazo takes two HTML pages and maps bits of one page into another page according to a rule-set.
It means you can have a designer create the design as static HTML and then you can map your dynamic site into that design without even modifying your site.
So you can style your PHP site or your Plone site without actually knowing either PHP or Plone.
Brilliant! We've used it on pretty much any site I've been involved with the last 4 years.

Diazo takes the same concepts and the same rule syntax as Deliverence, but it actually compiles the rules into XSLT.
You can then let nginx or apache do this mapping.
Or you can use the included WSGI server, or you can use it as a library inside your web framework.

Tool 1: caniusepython3
======================

This is both a command line tool and a website. https://caniusepython3.com/
It's not perfect, but it's helpful as a way to evaluate the application.
It told me it needed repoze.xmliter, but I think they have changed how it works slightly.
Now it only reports the packages that do not support Python 3, but where all dependencies support Python 3.
So in other words, caniusepython3 will now essentially recommend which package I should add Python 3 support to first.

In any case I started porting repoze.xmliter, and it will during testing use another module, collective.checkdocs that didn't support Python 3.

Adding Python 3 support to collective.checkdocs
===============================================

The collective.checkdocs source is on the Plone Collective svn server,
which is in read only mode, so I need to first migrate it to the collective repo on github.
I started that process (svn2git takes hours to run on that repository, it's huge)
and I mailed the original author to make sure that he is OK with it.

Once I got the OK from the original author I then added some simple tests to the module as it had no tests.

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

Tool 3: 2to3
============

I then ran 2to3 on the code to update things to Python 3.
It doesn't work perfectly, I need to clean up the imports manually.
I also need to add a from __future__ import print_function to get it to run under Python 2.

I add Python 3.2, 3.3 and 3.4 to the list of supported versions in setup.py.
I clean up things a bit, add a MANIFEST.in etc, makes sure Pyroma thinks the code is creamy, and release the module to PyPI.

Total time spent, including setting up Tox and then not using it anyway: Around 4 hours. The new version is released already.


Adding Python 3 support to repoze.xmliter
=========================================

repoze.xmliter is a wrapper to lxml that you can iterate over.
It will then give you chunks of byte strings of XML.
Not the most exiting module on PyPI, but interesting for this project, because it needs to handle both binary data and text!
This as we know, make it a Tricky Module to support Python 3.

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

Updating the documentation
==========================

The Diazo buildout includes a default test setup with Paste so you can develop your theme rules without nginx or Apache.
But Paste is not and will not be ported to Python 3.
The test setup also uses a lot of Paste apps, like urlmap and proxy, so I can't just switch it out for any old  WSGI server.
I needed one that used PasteDeploy.
A Python 3 compatible server designed to replace Paste's server exists in gearbox, but what about the apps?

Tool 5: Twitter!
================

I was discussing the issue on Twitter as I was preparing to port Paste's static and proxy apps.
The urlmap app was already ported as "rutter".
But then Ian Bicking pointed out that the apps I wanted to port already had been ported and was a part of WebOb!
However, it did not have any PasteDeploy entry points, so I needed to fix that.

webobentrypoints
================

So I started a package simply called "webobentrypoints".
As of today, it only contains PasteDeploy entry points for the static directory app and using the client app as a proxy,
because that's what was needed. I'll try to get time to add entry points for the other apps as well.

This took a long time because I neede to learn about the PasteDeploy entry points,
and I needed to re-learn WSGI which I hadn't looked at for years.
All in all this probably took 4-6 hours, of which maybe one was spent actually making the webobentrypoints package.

Less than 20 hours!
===================

That included porting collective.checkdocs, repoze.xmliter, Diazo and writing webobentrypoints.
Much of the time was not spent actually porting, but learning what the various modules actually did.


Conclusions
===========
