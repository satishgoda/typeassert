Intuitive and minimalistic type checking for Python objects
===========================================================

Getting Started
~~~~~~~~~~~~~~~~~~~~~~

The first snippet below checks that an object is a list of tuples, each tuple with an int and a string.
The second snippet shows how to decorate a function

.. code:: python

    activate_typeassert() # by default, typeassert is deactivated

    # takes as input an alternating list of objects, and their specification
    typeassert(12, 'int') 
    typeassert('foo', 'str', bar, 'str', 2.3, 'float')
    typeassert([(1, 'hello'), (2, 'world'), '[(int, str)..]')


Introduction
~~~~~~~~~~~~~~~~~~~~~~

Python is strongly, dynamically typed. This is fine, but at times when objects get too complicated, it can be a good thing to check for types.

typeassert allows simple checking of Python object formats. It has a very intuitive type system that mimics the way you define your own Python objects.

It can be particularily helpful when refactoring programs. Its performance has not been optimized and it is intended as a development-time type checking utility (disabled by default, and should probably be activated in tests only).


Related
~~~~~~~~~~~~~~~~~~~~~~

* https://www.python.org/dev/peps/pep-0484/
* http://mypy-lang.org/examples.html
* https://github.com/ceronman/typeannotations


Installation
~~~~~~~~~~~~~~~~~~~~~~

You can install typeassert with pip::

    pip install typeassert


Usage examples:
~~~~~~~~~~~~~~~~~~~~~~

.. code:: python

    from typeassert import typeassert, activate_typeassert

    # 'activates' typeassert. You need to explicitly call this,
    # else it will do nothing (by default, to save cpu time).
    # It is common to specify it in tests
    activate_typeassert()

    # typeassert only provides a single function, that takes two arguments.
    # the first one is the python object you want to test;
    # the second one is an 'object specification' (spec), that mimics the way
    # objects are represented in Python. This spec is a string representation of
    # a python object. For example, to check that 12 is an int or 'hello' is a str:
    typeassert(12, 'int')
    typeassert('hello', 'str')

    # A list of exactly 3 floats is specified as [float,float,float]
    typeassert([1.0,2.0,3.0], '[float,float,float]')

    # To match a variable-length list of ints, use [int..]
    typeassert([1,2,3], '[int..]')

    # To only check that it is a list (but not its content), use []
    typeassert([1, "deux"], '[]')

    # And so on for tuples,
    typeassert((1,2,3), '(int,int,int)')
    typeassert((1,2,3), '(int..)')
    typeassert((1, "deux"), '()')
    # dictionnaries,
    typeassert({11:2, 12: 3},  '{int:int}')
    typeassert({11:2, 12: 3},  '{}')
    # and sets.
    typeassert({11, 2},  '{int}')

    # Use the * wildcard if you don't want to check for a specific part
    typeassert({11:2, 12: "a", 13:(3,4)},  '{int:*}')

    # Further examples
    typeassert({11: (2,3), 12: (4,"5")}, '{ int: (int,*)}')
    typeassert([(2, "asdf"),(-12, "asfwe"),(1,"")], "[(int,str)..]")
    typeassert([(1, 'hello', 2.0)], "[(int, str, float)]")
