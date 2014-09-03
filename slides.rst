
Fear, uncertainty and doubt
===========================

Python 3 is a dead end!
-----------------------

Everything breaks!
------------------

There are no third-party modules!
---------------------------------

You will have to spend hundreds of man-hours!
---------------------------------------------

Python 3 is really a new language!
----------------------------------

Python 2 is good enough!
------------------------

Python 3 was a mistake!
-----------------------


Reality
=======


Python 3 is a dead end!
-----------------------

* All new features go into Python 3.

* Some available as libraries, but not all.

A common argument against Python 3 is that it makes Python 2 a dead end.
Most people that ask for a Python 2.8 want all new Python 3 features to be backported to Python 2.
So the reality is that it's Python 2 that is the dead end.

Everything breaks!
------------------

It's hard to argue with this, because although not everything breaks,
you do in general have to make changes to your code.
I've so far only found one module that actually ran on Python 3 without modifications, and it was very simple.
And even then, the tests broke and I had to fix them.


There are no third-party modules!
---------------------------------

Include relevant Python 3 graph here.


You will have to spend hundreds of man-hours!
---------------------------------------------

Well, this really depends on what you are porting, and how much of course.
But I have ported a whole bunch of libraries, and perhaps I have spent hundreds of hours on this.
Well, no, not perhaps, I have spent hundreds of man hours on it.
But these were some really hard libraries to port, and I ported them to Python 3.0 or 3.1,
which are much harder to port to than Python 3.3 and later.
I also needed them to run on Python 2.5 or even Python 2.4, adding a whole extra player of problems.
I'm going to look into this later.


Python 3 is really a new language!
----------------------------------

If it was a new language, then you would get confused when reading Python 3 code.
You would not be entirely sure what the code would do, as bits of it wouldn't make sense.
That is not the case, I promise.
Python 3 code looks exactly like Python 2 code, with some minor differences.
The most telltale sign is usually that print is a function now.


Python 2 is good enough
-----------------------

Well, I can't argue with that one.
I agree, it is good enough.
But Python 3 *is* better.


Python 3 was a mistake
----------------------

There was several mistakes that could only be fixed by breaking backwards compatibility.

* Floor division

* Exception syntax

* Unicode

Loads of other languages never break backwards compatibility, they just load more and more stuff on,
making the language more and more complex.
Is that really what we would like to do to Python?
C++ has 84 keywords, 10 of them was new in C++ 11.
Python has 33, 4 of them is new in Python 3 (False, True, None and nonlocal) and two taken away (exec and print).
Recently there has been reports that Python is now the number one language used in beginners programming classes on universities.
This is the reason for that.
And if we want Python to continue to be everyones favourite language, that simplicity must remain.
So I don't think Python 3 was a mistake.


Most changes are not so bad
===========================

My interest in Python 3 started at EuroPython 2007.
Guido held a keynote about Python 3,
and explained that writing code that would run on both Python 2 and Python 3 would be very complicated.
And then he listed all the differences, and it didn't seem that bad,
so I asked myself if it really was that hard to write code that ran on both versions?
And the answer is: No, it's not that hard!
There are a lot of changes that are indeed backwards incompatible, but which allows forwards compatibility.
For example, take the changes to exception syntax.

.. code:: python

    except Exception, e:

Turned into

.. code:: python

    except Exception as e:

The first syntax is not allowed in Python 3.
But, the second syntax is allowed in Python 2.6 and 2.7.
That means that you can perfectly well write code that runs on both Python 2 and Python 3 using the new syntax,
as long as you don't need to support versions before Python 2.6.

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


Tools that can help you
=======================


Practical Experiences
=====================


Conclusions
===========
