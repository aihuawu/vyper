.. index:: type

.. _types:

Types
#####

Vyper is a statically typed language. The type of each variable (state and local) must be specified or at least known at compile-time. Vyper provides several elementary types which can be combined to form complex types.

In addition, types can interact with each other in expressions containing operators.

.. index:: ! value

Value Types
===========

The following types are also called value types because variables of these
types will always be passed by value, i.e. they are always copied when they
are used as function arguments or in assignments.

.. index:: ! bool, ! true, ! false

Boolean
-------

**Keyword:** ``bool``

A boolean is a type to store a logical/truth value.

Values
******

The only possible values are the constants ``True`` and ``False``.

Operators
*********

====================  ===================
Operator              Description
====================  ===================
``x not y``           Logical negation
``x and y``           Logical conjunction
``x or y``            Logical disjunction
``x == y``            Equality
``x != y``            Inequality
====================  ===================

Short-circuiting of boolean operators (``or`` and ``and``) is consistent with
the behavior of Python.

.. index:: ! int128, ! int, ! integer

Signed Integer (128 bit)
------------------------

**Keyword:** ``int128``

A signed integer (128 bit) is a type to store positive and negative integers.

Values
******

Signed integer values between -2\ :sup:`127` and (2\ :sup:`127` - 1), inclusive.

Interger literals cannot have a decimal point even if the decimal value is zero. For example, ``2.0`` cannot be interpreted as an integer.

Operators
*********

Comparisons
^^^^^^^^^^^

Comparisons return a boolean value.

==========  ================
Operator    Description
==========  ================
``x < y``   Less than
``x <= y``  Less than or equal to
``x == y``  Equals
``x != y``  Does not equal
``x >= y``  Greater than or equal to
``x > y``   Greater than
==========  ================

``x`` and ``y`` must be of the type ``int128``.

Arithmetic Operators
^^^^^^^^^^^^^^^^^^^^

=============  ======================
Operator       Description
=============  ======================
``x + y``      Addition
``x - y``      Subtraction
``-x``         Unary minus/Negation
``x * y``      Multiplication
``x / y``      Division
``x**y``       Exponentiation
``x % y``      Modulo
=============  ======================

``x`` and ``y`` must be of the type ``int128``.

.. index:: ! unit, ! uint256

Unsigned Integer (256 bit)
--------------------------

**Keyword:** ``uint256``

An unsigned integer (256 bit) is a type to store non-negative integers.

Values
******

Integer values between 0 and (2\ :sup:`256`-1).

Interger literals cannot have a decimal point even if the decimal value is zero. For example, ``2.0`` cannot be interpreted as an integer.

.. note::
    Integer literals are interpreted as ``int128`` by default. In cases where ``uint256`` is more appropriate, such as assignment, the literal might be interpreted as ``uint256``. Example: ``_variable: uint256 = _literal``. In order to explicitly cast a literal to a ``uint256`` use ``convert(_literal, uint256)``.

Operators
*********

Comparisons
^^^^^^^^^^^

Comparisons return a boolean value.

==========  ================
Operator    Description
==========  ================
``x < y``   Less than
``x <= y``  Less than or equal to
``x == y``  Equals
``x != y``  Does not equal
``x >= y``  Greater than or equal to
``x > y``   Greater than
==========  ================

``x`` and ``y`` must be of the type ``uint256``.

Arithmetic Operators
^^^^^^^^^^^^^^^^^^^^

===========================  ======================
Operator                     Description
===========================  ======================
``x + y``                    Addition
``x - y``                    Subtraction
``x * y``                    Multiplication
``x / y``                    Division
``x**y``                     Exponentiation
``x % y``                    Modulo
===========================  ======================

``x``, ``y`` and ``z`` must be of the type ``uint256``.

Decimals
--------

**Keyword:** ``decimal``

A decimal is a type to store a decimal fixed point value.

Values
******

A value with a precision of 10 decimal places between -2\ :sup:`127` and (2\ :sup:`127` - 1).

In order for a literal to be interpreted as ``decimal`` it must include a decimal point.

Operators
*********

Comparisons
^^^^^^^^^^^

Comparisons return a boolean value.

==========  ================
Operator    Description
==========  ================
``x < y``   Less than
``x <= y``  Less or equal
``x == y``  Equals
``x != y``  Does not equal
``x >= y``  Greater or equal
``x > y``   Greater than
==========  ================

``x`` and ``y`` must be of the type ``decimal``.

Arithmetic Operators
^^^^^^^^^^^^^^^^^^^^

=============  ==========================================
Operator       Description
=============  ==========================================
``x + y``      Addition
``x - y``      Subtraction
``-x``         Unary minus/Negation
``x * y``      Multiplication
``x / y``      Division
``x % y``      Modulo
=============  ==========================================

``x`` and ``y`` must be of the type ``decimal``.

.. _address:

Address
-------

**Keyword:** ``address``

The address type holds an Ethereum address.

Values
******

An address type can hold an Ethereum address which equates to 20 bytes or 160 bits. Address literals must be written in hexadecimal notation with a leading ``0x``. All addresses must be `checksummed <https://github.com/ethereum/EIPs/blob/master/EIPS/eip-155.md>`_.

.. _members-of-addresses:

Members
^^^^^^^

===============  =========================================================
Member           Description
===============  =========================================================
``balance``      Query the balance of an address. Returns ``uint256``.
``codehash``     Returns the ``bytes32`` keccak of the code at an address, or ``EMPTY_BYTES32`` if the account does not currently have code.
``codesize``     Query the code size of an address. Returns ``int128``.
``is_contract``  Query whether it is a contract address. Returns ``bool``.
===============  =========================================================

Syntax as follows: ``_address.<member>``, where ``_address`` is of the type ``address`` and ``<member>`` is one of the above keywords.

.. note::

    Operations such as ``SELFDESTRUCT`` and ``CREATE2`` allow for the removal and replacement of bytecode at an address. You should never assume that values of address members will not change in the future.

32-bit-wide Byte Array
----------------------

**Keyword:** ``bytes32``
This is a 32-bit-wide byte array that is otherwise similar to byte arrays.

**Example:**
::

    # Declaration
    hash: bytes32
    # Assignment
    self.hash = _hash

Operators
*********

====================================  ============================================================
Keyword                               Description
====================================  ============================================================
``keccak256(x)``                      Return the keccak256 hash as bytes32.
``concat(x, ...)``                    Concatenate multiple inputs.
``slice(x, start=_start, len=_len)``  Return a slice of ``_len`` starting at ``_start``.
====================================  ============================================================

Where ``x`` is a byte array and ``_start`` as well as ``_len`` are integer values.

.. index:: !bytes

Fixed-size Byte Arrays
----------------------

**Keyword:** ``Bytes``

A byte array with a fixed size.
The syntax being ``Bytes[maxLen]``, where ``maxLen`` is an integer which denotes the maximum number of bytes.
On the ABI level the Fixed-size bytes array is annotated as ``bytes``.

Bytes literals may be given as bytes strings, hexadecimal, or binary.

.. code-block:: python

    bytes_string: Bytes[100] = b"\x01"
    hex_bytes: Bytes[100] = 0x01
    binary_bytes: Bytes[100] = 0b00000001

.. index:: !string

Fixed-size Strings
------------------

**Keyword:** ``String``
Fixed-size strings can hold strings with equal or fewer characters than the maximum length of the string.
On the ABI level the Fixed-size bytes array is annotated as ``string``.

.. code-block:: python

    example_str: String[100] = "Test String"

.. index:: !reference

Reference Types
===============

Reference types do not fit into 32 bytes. Because of this, copying their value is not as feasible as
with value types. Therefore only the location, i.e. the reference, of the data is passed.

.. index:: !arrays

Fixed-size Lists
----------------

Fixed-size lists hold a finite number of elements which belong to a specified type.

Syntax
******

Lists can be declared with ``_name: _ValueType[_Integer]``. Multidimensional lists are also possible.

.. code-block:: python

    # Defining a list
    exampleList: int128[3]

    # Setting values
    exampleList = [10, 11, 12]
    exampleList[2] = 42

    # Returning a value
    return exampleList[0]

.. _types-struct:

Structs
-------

Structs are custom defined types that can group several variables.

Syntax
******

Struct members can be accessed via ``struct.argname``.

.. code-block::

    # Defining a struct
    struct MyStruct:
        value1: int128
        value2: decimal

    # Declaring a struct variable
    exampleStruct: MyStruct = MyStruct({value1: 1, value2: 2})

    # Accessing a value
    exampleStruct.value1 = 1


.. index:: !mapping

Mappings
--------

Mappings are `hash tables <https://en.wikipedia.org/wiki/Hash_table>`_ that are virtually initialized such that every possible key exists and is mapped to a value whose byte-representation is all zeros: a type's :ref:`default value <types-initial>`.

The key data is not stored in a mapping, instead its ``keccak256`` hash used to look up a value. For this reason mappings do not have a length or a concept of a key or value being "set".

.. note::
    Mappings are only allowed as state variables.

Syntax
******

Mapping types are declared as ``HashMap[_KeyType, _ValueType]``.

* ``_KeyType`` can be any base or bytes type. Mappings, interfaces or structs are not support as key types.
* ``_ValueType`` can actually be any type, including mappings.


.. code-block:: python

   # Defining a mapping
   exampleMapping: HashMap[int128, decimal]

   # Accessing a value
   exampleMapping[0] = 10.1


.. note::

    Mappings have no concept of length and so cannot be iterated over.

.. index:: !initial

.. _types-initial:

Initial Values
==============

Unlike most programming languages, Vyper does not have a concept of ``null``. Instead, every variable type has a default value. To check if a variable is empty, you must compare it to the default value for it's given type.

To reset a variable to it's default value, assign to it the built-in ``empty()`` function which constructs a zero value for that type.

.. note::

    Memory variables must be assigned a value at the time they are declared.

Here you can find a list of all types and default values:

.. list-table:: Default Variable Values
   :header-rows: 1

   * - Type
     - Default Value
   * - ``bool``
     - ``False``
   * - ``int128``
     - ``0``
   * - ``uint256``
     - ``0``
   * - ``decimal``
     - ``0.0``
   * - ``address``
     - ``0x0000000000000000000000000000000000000000``
   * - ``bytes32``
     - ``'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00'``

.. note::
    In ``Bytes`` the array starts with the bytes all set to ``'\x00'``

.. note::
    In reference types all the type's members are set to their initial values.


.. _type_conversions:

Type Conversions
================

All type conversions in Vyper must be made explicitly using the built-in ``convert(a, b)`` function. Currently, the following type conversions are supported:

.. list-table:: Basic Type Conversions
   :header-rows: 1

   * - Destination Type (b)
     - Input Type (a.type)
     - Allowed Inputs Values (a)
     - Additional Notes
   * - ``bool``
     - ``bool``
     - ``—``
     - Do not allow converting to/from the same type
   * - ``bool``
     - ``decimal``
     - ``MINNUM...MAXNUM``
     - Has the effective conversion logic of: ``return (a != 0.0)``
   * - ``bool``
     - ``int128``
     - ``MINNUM...MAXNUM``
     - Has the effective conversion logic of: ``return (a != 0)``
   * - ``bool``
     - ``uint256``
     - ``0...MAX_UINT256``
     - Has the effective conversion logic of: ``return (a != 0)``
   * - ``bool``
     - ``bytes32``
     - ``(0x00 * 32)...(0xFF * 32)``
     - Has the effective conversion logic of: ``return (a != 0x00)``
   * - ``bool``
     - ``Bytes``
     - ``(0x00 * 1)...(0xFF * 32)``
     - Has the effective conversion logic of: ``return (a != 0x00)``
   * -
     -
     -
     -
   * - ``decimal``
     - ``bool``
     - ``True / False``
     - Result will be ``0.0`` or ``1.0``
   * - ``decimal``
     - ``decimal``
     - —
     - Do not allow converting to/from the same type
   * - ``decimal``
     - ``int128``
     - ``MINNUM...MAXNUM``
     -
   * - ``decimal``
     - ``uint256``
     - ``0...MAXDECIMAL``
     -
   * - ``decimal``
     - ``bytes32``
     - ``(0x00 * 32)...(0xFF * 32)``
     -
   * - ``decimal``
     - ``Bytes``
     - ``(0x00 * 1)...(0xFF * 32)``
     -
   * -
     -
     -
     -
   * - ``int128``
     - ``bool``
     - ``True / False``
     - Result will be ``0`` or ``1``
   * - ``int128``
     - ``decimal``
     - ``MINNUM...MAXNUM``
     - Only allow input within ``int128`` supported range, truncates the decimal value
   * - ``int128``
     - ``int128``
     - —
     - Do not allow converting to/from the same type
   * - ``int128``
     - ``uint256``
     - ``0...MAXNUM``
     -
   * - ``int128``
     - ``bytes32``
     - ``(0x00 * 32)...(0xFF * 32)``
     -
   * - ``int128``
     - ``Bytes``
     - ``(0x00 * 1)...(0xFF * 32)``
     -
   * -
     -
     -
     -
   * - ``uint256``
     - ``bool``
     - ``True / False``
     - Result will be ``0`` or ``1``
   * - ``uint256``
     - ``decimal``
     - ``0...MAXDECIMAL``
     - Truncates the ``decimal`` value
   * - ``uint256``
     - ``int128``
     - ``0...MAXNUM``
     -
   * - ``uint256``
     - ``uint256``
     - —
     - Do not allow converting to/from the same type
   * - ``uint256``
     - ``bytes32``
     - ``(0x00 * 32)...(0xFF * 32)``
     -
   * - ``uint256``
     - ``Bytes``
     - ``(0x00 * 1)...(0xFF * 32)``
     -
   * -
     -
     -
     -
   * - ``bytes32``
     - ``bool``
     - ``True / False``
     - Result will be either ``(0x00 * 32)`` or ``(0x00 * 31 + 0x01)``
   * - ``bytes32``
     - ``decimal``
     - ``MINDECIMAL...MAXDECIMAL``
     - Has the effective behavior of multiplying the ``decimal`` value by the decimal divisor ``10000000000`` and then converting that signed *integer* value to a ``bytes32`` byte array
   * - ``bytes32``
     - ``int128``
     - ``MINNUM...MAXNUM``
     -
   * - ``bytes32``
     - ``uint256``
     - ``0...MAX_UINT256``
     -
   * - ``bytes32``
     - ``bytes32``
     - —
     - Do not allow converting to/from the same type
   * - ``bytes32``
     - ``Bytes``
     - ``(0x00 * 1)...(0xFF * 32)``
     - Left-pad input ``Bytes`` to size of ``32``


.. index:: !conversion
