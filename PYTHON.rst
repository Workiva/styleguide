Workiva Python Style Guide
=============================

Workiva's python style guide is based primarily off of the PEP8_ and `Google
Style Guide`_. As with all style guides, there is room for interpretation, so
this Workiva Style Guide is intended to fill those gaps to provide a consistent
look across the Python code base.

As with all style guides, the key to this one is readability. As the PEP8
guidelines say about readability_, "A Foolish Consistency is the Hobgoblin 
of Little Minds." Your goal should always be to make your code as easy to 
read and understand as possible above all else.

Docstrings:
+++++++++++

Docstrings are documentation for Python. The current standard for Workiva is
`Sphinx-Style Docstrings`_. They are almost as important as the code itself,
providing a spread of knowledge about the code base as well as making it easier
to track down and understand the flow of our software.

A Sample Docstring:
-------------------

.. code:: python

  def function(attr1, attr2=None, attr3=0)
      """
      A brief summary of the function.

      A longer description of the function which goes into more detail, and
      can span multiple lines.

      :param attr1: The first attribute, we will assume it is a string.
      :type attr1: str
      :param attr2: The second attribute, we will assume it is either a class
          called Object or None. Defaults to None.
      :type attr2: :class:`~path.to.Object` or None
      :param attr3: The third attribute, we will assume it is a list of
          different class objects.
      :type attr3: list [:class:`~path.to.other.Object`]
      :raises ExceptionClass: An exception which may in some cases be
          raised by this function.
      :return: The Object modified to include the string manipulated somehow by
           the other object.
      :rtype class:`~path.to.Object`
      """
      ...

Docstrings can get far more complicated, but this can serve as a basic primer.
In addition, there are the `Google-Style Docstrings`_, but they are not
currently supported by JetBrains products or the current release of Sphinx. This
will likely change in the future, but for now the Sphinx standard is preferred.

For parameters and returns of type list, it is helpful to indicate in
the docstring the data type of the objects contained in the list.  This may not
be practical in some cases especially if the list contains mixed data types.
If a parameter or a return is of type dict and the contents of the dict are
all of the same type, it is helpful to note the data type of the contents
of the dict in the same way.

Docstrings for Modules:
-----------------------
Docstrings for modules are optional, but are usually very helpful to your fellow
coders. This is a docstring that is at the beginning of a module, before the
imports. It should provide a summary of what the module is made to accomplish,
as well as any extra global information that would be helpful. For example:

.. code:: python

    """
    This module is a group of utility functions for some package.
    """
    from math import pow

Docstrings for Classes:
-----------------------
Docstrings for classes are also optional, but very frequently useful. They allow
you to explain the purpose of the whole class without someone needing to read
and try to understand the logic behind the grouping of the class's methods. You
can also document the class attributes, which can avoid a lot of confusion from
the intent and use of the attributes.

.. code:: python

    class ArbitraryClass(object):
    """
    This is an arbitrary class that contains a set of functions to initialize
    and manipulate an arbitrary object.
    """
    #: Attribute set to None, but used as a dictionary. Set to none to make
    #: it be localized to an individual instance instead of global.
    attribute_one = None
    #: Attribute set to an empty list so that the list will be shared between
    #: all instances of the ArbitraryClass objects.
    attribute_two = []
    ...

Docstrings for Methods:
-----------------------
Docstrings on overloads of builtins on classes are optional.  This applies to
methods like __init__() and __call__().  Docstrings are not required in this
case because these methods generally do just one thing and are well understood.
The heavy lifting for the class is usually delegated to other methods.

Docstrings for public class methods should have a full docstring and meet
the standard requirements for function docstrings.

Trivial helper functions on the class do not require a docstring.

A private helper method should be annotated with a docstring with a one
line summary if it is not-trivial.


Docstrings for a function without a return:
-------------------------------------------
When documenting some functions, they have no return value to assign to the
return or rtype components. Truthfully, all python functions implicitly return a
None unless overridden, so all functions have a return type. For the return text
it is often a good place to say what the function or method is modifying, if it
has a side effect, or what the state of the program will be after the code
executes. For example:

.. code:: python

    ...
    :return: No direct return, but appends to the output buffer.
    :rtype: None
    """

Docstrings and Unit Tests:
--------------------------
When running unit tests, programs like nose will actually introspect the first
line of the docstring of a test method and output it if the test fails. To make
this more useful, it is suggested you format your testing docstrings in a way
that makes the most of this feature. For example:

.. code:: python

    class TestRunner(unittests.TestCase):
        def first_test(self):
        """
        This should be a short one line description

        This can be a longer summary if needed.
        """
        ...

Since the elements being passed into a unit test rarely need explaining, the
parameter block for a unittest's docstring is not required.

Non-trivial helper functions for a unit test should be annotated with a
docstring containing a single-line summary of the function's behavior.

Outdated Docstrings:
--------------------------
Docstrings can become outdated as our documentation format changes or as our
codebase evolves.  The usefulness of these docstrings degrades over time if
we do not make the effort to keep them up to date.

Frequent symptoms of an outdated docstring are:

- parameter descriptions that use an '@' symbol.  e.g. @param.
- Functions whose function signature has expanded without a corresponding
  expansion of the docstring. For example, the function may take four
  arguments while the docstring only describes two of them.
- Missing data types for parameters.
- Missing data type for the return.
- Missing description of function side effects when a function has no
  explicit return.

If a developer makes a code change in a function, he or she should check to
make sure that the docstring is up to date.  The docstring for the function
should be updated (if necessary) as part of the work of modifying the function.

**Exception:** If a developer is working on an expedited ticket, we do want to
impede progress on this by insisting on updating outdated docstrings within
the scope of that ticket.  Instead, the developer should spin off a tech-debt
ticket to cover updating outdated docstrings for the functions that their
code touches.  They can return to the tech debt ticket at a later time,
after the expedited ticket has been addressed.

Code Comments:
++++++++++++++

Commented Out Code:
-------------------

If a block of code is commented out, it should always be accompanied with the
reasoning for why it was commented out instead of deleted. Commented out code
without a justification is a landmine of confusion waiting to happen. Best
option is to just delete the code, as the code changes still exist in the
commit history, and are most likely not needed still in the code base.

Bad:

.. code:: python

  ...
  # result = Class.function(param1=0)
  ...

Better:

.. code:: python

  ...
  # result = Class.function(param1=0)
  # Above code isn't currently needed, but might be useful if X happens in the
  # future.
  ...

Best:

.. code:: python

  ...

BUG-COMPAT Tags:
----------------

Sometimes, coding around a bug is unavoidable. In that case, the offending code
is required to be annotated with a BUG-COMPAT tag, including the JIRA reference
of the ticket to correct the bug you are coding around. For example:

.. code:: python

  obj = Object()
  # BUG-COMPAT: JIRA-1234 Need to set value_one explicitly to None because of
  # reason x.
  obj.value_one = None

TODO Tags:
----------

TODO tags are used to give you guidance on work that will need to be done in
the future. Where possible, try to annotate these with a ticket number. While
it isn't as essential to tag with a ticket as a BUG-COMPAT tag, it is still
very helpful.

.. code:: python

  # TODO: Add defensive error handling to this call, ticket: YOUR-1234
  result = function()

MAGIC-NUMBER and MAGIC-STRING Tags:
-----------------------------------

Magic numbers or strings are best avoided, but that is not always possible.
When they are unavoidable, document them with a MAGIC-NUMBER or MAGIC-STRING
tag so that the rationale for including them is not lost.

.. code:: python

  # MAGIC-NUMBER: 1.0 is used because of reason x. Any other value will cause
  # an error to be thrown.
  result = function(parameter_one=1.0, parameter_two=value_two)

FUTURE Tags:
------------
Future tags are used to mark code that will need to change should a future
condition be met. It can be useful for future-proofing regions of code, as well
as passing design ideas onto future developers working in the code base.

.. code:: python

    ...
    # FUTURE: Should the API support Decimal instead of float we no longer need
    # to worry about casting these values to Decimal
    converted_value = Decimal(value)
    ...

Import Statements:
++++++++++++++++++

`Import statements`_ are one of the most common places that cruft gets left in
code. Unused imports, poorly grouped imports, and lack of organization can make
the import block difficult to read and confusing. These guidelines can help to
prevent your import section from becoming a confusing and unreadable block.

Location of Import Statements:
------------------------------

Import statements should always be at the module level, unless there is a
justification to put them at the functional level; to avoid circular imports,
for example. The primary reason for this is improved performance when executing
in GAE if you avoid function-scope imports.

Relative Imports:
-----------------

Relative imports allow you to import without needing to know the full path to
the code you are importing. This can be immensely useful, but can often be hard
to understand for someone new to a code base or someone trying to refactor. Due
to this, relative imports are best avoided. They should be avoided completely if
you are working on a python library, as it is likely to cause naming conflicts 
as systems grow more complex.

Grouping Import Statements:
---------------------------

Code imports should be split into three groups:

* Python's Built-in libraries
* Third Party libraries (ie: Google's AppEngine, Numpy, ReportLab, etc.)
* Library-local imports

For example:

.. code:: python

  # Python Imports
  import logging
  from datetime import datetime

  # 3rd Party Imports
  import numpy
  from google.appengine.ext import ndb

  # Local Imports
  from local_library import local_module

The comment lines are optional, but a single blank space is required between
groups.

Import Conventions:
-------------------
Often you will need to import more than one thing from a module, for example:

.. code:: python

    from math import radians, degrees

While this is fine for python built-in functions, and even for some 3rd party
libraries, it is best to try to stick to one import per line. Especially for
Workiva code, as our code base is frequently changing and improving we have
components that move, change names or get merged together.

Though one import per line sounds doable, what about those cases where you need
to import three, four, or more components? If you were to import each on an
individual line, it would make your import block be giant.

Well, in those cases consider importing the module itself, then using the
namespace.object notation to access individual elements within it. For example:

.. code:: python

    from math import radians, degrees, exp, log, ceil, floor

This gets out of hand pretty quickly, it is better to just import the module,
and reference the namespace like this:

.. code:: python

    import math
    ...
    result = math.ceil(variable)
    ...

This is even more important with Workiva-based code libraries, as they are
more in flux, and therefore more likely to be missed during a refactor.
The following format is recommended, especially for imports that cross
repository codebase boundaries:

.. code:: python

    from wf import crypto
    from wf import datastore
    from wf import utils

The 'one import per line' convention is a suggestion, not a mandate.
Sometimes a short double or triple import is simple and clear.  When in
doubt as to the correct import syntax, consider and apply the
principles of readability, clarity and transparency of intention.

Constants Definitions:
++++++++++++++++++++++

Constants should be defined at the module level whenever possible, located after
the imports, before the first class or function. They should be named in all
caps with underscores.

.. code:: python

  ...
  from local_library import local_module


  NON_BREAK_SPACE_UNICODE = u'\xa0'


  def function1():
  ...

Per PEP8's `blank lines`_ spec, there should be two newlines between each item
at the module level, so you want two blank lines before the constants block and
two blank lines after the constants and before the first class or function
definition.

Ambiguous and Hard to Read Code:
++++++++++++++++++++++++++++++++

Sometimes, python allows statements and formatting that while valid, can be
misleading to others reading the code. These practices are best avoided for the
sake of clarity.

Chained Assignments
-------------------
Even though a statement like this will save a couple lines:

.. code:: python

    first = second = third = 0

Down the road when things get more complex, this pattern can get confusing, and
is best avoided.

Chained Evaluations
-------------------
In the same vein as chained assignments, chained evaluations can save a little
bit of space, but sometimes at the cost of clarity. For example:

.. code:: python

    if x_val < y_val < z_val:

This can often be confusing because when read it is not readily apparent what
the range of values would be to make that chained inequality result to true or
false would be.

Simplistic cases are allowed though, as they can actually make the code clearer
and easier to understand, such as:

.. code:: python

    if 0 < true_range < 10:

This is because it very clearly conveys what the range of values that are valid
would be, and is actually clearer than breaking the statement into two
components.

Multi-Line Statements:
++++++++++++++++++++++

Often in code, we have a single line that just won't fit in the 79_ character
limit. There are many ways in Python to resolve this, but at Workiva we use
these heuristics to help maintain consistency in the code. As with the rest of
this style guide, readability_ is king.

Multi-Line Dictionaries, Lists, and Tuples:
-------------------------------------------
For simple and empty definitions, a single line statement should be sufficient.

.. code:: python

  a_dict = {}
  a_dict = {'simple': 0.0}
  a_list = []
  a_list = ['item1', 'item2']
  a_tuple = ()
  a_tuple = (1, 2, 3)

When things get more complicated, make sure that your opening and closing
brackets match up with start of the first line, for example:

.. code:: python

  dictionary = {
      'list': [
          'alpha', 'beta', 'gamma', 'delta', 'epsilon', 'zeta', 'eta', 'theta',
          'iota', 'kappa', 'lamda', 'mu', 'nu', 'xi', 'omikron', 'pi', 'rho',
          'sigma', 'tau', 'upsilon', 'phi', 'chi', 'psi', 'omega'
      ],
      'tuple': (
          10000000000, 20000000000, 30000000000, 40000000000, 50000000000,
          60000000000, 70000000000, 80000000000, 90000000000
      )
  }

Matching the closing bracket of the dictionary, list, or tuple with the opening
statement will make it much easier to detect where an element's contents finish.
The idea is to open or close a context on its own line to prevent any confusion
of what scope any individual line of the multiline statement belongs to.

Multi-Line Function Calls:
--------------------------

Often, when making a function call, arguments provided are impossible to fit
onto a single line. In order to make the code readable, and avoid \\'s, use the
opening and closing brackets to your advantage. They create a context in which
you can space things neatly, much like a multi-line dictionary, list, or tuple.

.. code:: python

  result = Class.function(
      attribute_1=value_1,
      attribute_2={
          'dictionary_key_0': "dictionary_value_0",
          'dictionary_key_1': "dictionary_value_1", 'dictionary_key_2': 0,
          'dictionary_key_3': "dictionary_value_3",
          'dictionary_key_4': 0.0, 'dictionary_key_5': "dictionary_value_5",
          'dictionary_key_6': None, 'dictionary_key_7': {},
          'dictionary_key_8': "dictionary_value_8",
          'dictionary_key_9': ""
      },
      attribute_3={
          'dictionary_key_0': function0, 'dictionary_key_1': function1,
          'dictionary_key_2': function0, 'dictionary_key_3': function2,
          'dictionary_key_4': function0
      },
      attribute_4=True

  )

This format makes it much easier to fit all of the parameters of a call in an
easy to read structure, clearly delineating the end of the parameters. It also
makes it easier to explicitly name the values being set, a good practice to
prevent values being incorrectly set if/when a function signature changes.

Multi-Line Function Definitions:
--------------------------------

Along with multi-line function calls, often our function signatures will be too
large to fit on a single line, especially with default parameters. When you have
a long function definition, line up the following lines with the start of the
first parameter, for example:

.. code:: python

  def function(parameter_one, parameter_two=0, parameter_three=0.0,
               parameter_four="", parameter_five=None):

This makes it easy to line up the parameter block for easier reading.

Multi-Line if/elif/with Statements:
-----------------------------------

Multi-line if/elif/with statements behave similarly to multi-line function
calls, but with a few differences. The first and foremost is problem caused by
the fact that the phrase 'if (' is exactly four characters. Often this leads to
if statments that look like this:

.. code:: python

  if (fact_one == 'one' and
      fact_two == 'two'):
      code...

Which will often throw an E128 errors when running through tools like Flake8.
The reason for this is that because the second line both lines up with the start
of the tuple as well as the beginning of the indented block of the if
statement's body. To prevent this, wrap complicated if/elif/with statements in
an extra layer of parenthesis and line up the lines with the beginning of the
inner parenthesis.

.. code:: python

  if ((fact_one == 'one' and
       fact_two == 'two')):
      code...

This will also make it quick and easy to read what the conjunctions are
(ie: and, or, and not, or not), as well as allow for easier reading on very
complicated if statements. For example:

.. code:: python

  if ((fact_one > 1 and
       ((fact_two == 'remove' or
         fact_three != 'true')))):
      code...

This style, while it looks like it has an overabundance of parenthesis, will
allow for quick understanding of the logical groupings used for an if or elif.
Rarely will an example be as simple as the above sample, but with the extra
wrapping you will easily be able to fit the most complicated if statements in
the character limit in a readable and understandable way. For example:

.. code:: python

  if ((len(children) > 1 and
       ((str(node.get('changeStatus')).lower() == 'remove' or
         str(node.get('ignoreChildren')).lower() != 'true')))):
      code...

This code block fits within the 79_ character limit, while still being
readable and logically grouped.

This style also allows for the use of as renames in with statements. To use a
with mock.patch as an example:

.. code:: python

  with ((patch(
             'path.to.mock.for.a.test',
              return={'item_one': None, 'item_two': None}
         ))) as test:
      ...code...

The extra parenthesis allows you to group the patch call all together and use
the same pattern you would normally use for a multi-line function call inside
the with declaration, and allows for an easy rename without causing problems
with under-indentation errors. It makes code like this easier to understand and
read through quickly while staying under the character limit.

Multi-Line List Comprehensions:
-------------------------------

List comprehensions are a very useful tool in Python, but can often get large
and complicated. To make them more readable, there are a couple of guidelines
you can follow.

First, try to break logical components onto their own line inside the list
comprehension's scope like so:

.. code:: python

    generated_list = [
        function(value) for value in value_list
        if value is not None
        and value.is_valid()
    ]

The first line should have whatever the elements of the list are being set to,
as well as the for component of the comprehension if it will fit. If not, it
should be alone on the next line. Next, if there are any conditionals on the
assignment, they should be clearly broken up, usually on their own lines, so
that each line is read as an atomic component.

Multi-Line String Constants:
----------------------------

Often our logging statements or other string constants grow too large to fit on
as single line. There are two simple methods to resolving this, and which you
use is partially stylistic and partially performance based.

The basic case is to simply break the line at the 79 character mark and continue
on the next line.

.. code:: python

  logging.info(
    "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Aliquam eu nunc "
    "gravida, faucibus felis a, aliquam justo. Pellentesque faucibus nisl eu "
    "faucibus malesuada. Vivamus vestibulum, magna eu scelerisque sagittis, "
    "dui urna tempus libero, sed pellentesque nulla felis luctus felis. "
    "Phasellus ac tortor dignissim, euismod nibh luctus, euismod ligula."
  )

This method of continuing is preferred to using concatenation as it offers
superior performance.

Unfortunately, the concatenation operation for strings in python is bit slow
especially compared to continuation, so if a long string is going to be used
often, it is usually more performant to use the join method of strings.
Thus a continuation is preferred to a join, and a join is preferred to +
concatenation.


.. code:: python
  for item in foo:
    arbitrary_list.append(item.generate_string())

  # other code...

  logging.info(
    "".join(arbitrary_list)
  )

While it would work to break the string exactly at the 77nd character mark,
it is better to break it at a logical point that makes it continue to be
readable. Remember the goal is to make the code easier to read, not to save
space.


.. _`Google Style Guide`: http://google-styleguide.googlecode.com/svn/trunk/pyguide.html
.. _PEP8: http://legacy.python.org/dev/peps/pep-0008/
.. _79: http://legacy.python.org/dev/peps/pep-0008/#maximum-line-length
.. _readability: http://legacy.python.org/dev/peps/pep-0008/#a-foolish-consistency-is-the-hobgoblin-of-little-minds
.. _`blank lines`: http://legacy.python.org/dev/peps/pep-0008/#blank-lines
.. _`Sphinx-Style Docstrings`: https://pythonhosted.org/an_example_pypi_project/sphinx.html#function-definitions
.. _`Google-Style Docstrings`: http://sphinxcontrib-napoleon.readthedocs.org/en/latest/example_google.html
.. _`Import statements`: http://legacy.python.org/dev/peps/pep-0008/#imports
