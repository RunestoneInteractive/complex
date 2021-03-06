..  Copyright (C)  Jan Pearce
    This work is licensed under the Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License. To view a copy of this license, visit http://creativecommons.org/licenses/by-nc-sa/4.0/.

.. _CA_13:

CA Tables
---------

.. _6.12:

The function we have so far works if the CA is “totalitic", which means that the rules only depend on the sum of the neighbors. But most rules also depend on which neighbors are on and off. For example, 100 and 001 have the same sum, but for many CAs, they would yield different results.

We can make ``step2`` more general using a window with elements [:math:`4`, :math:`2`, :math:`1`], which interprets the neighborhood as a binary number. For example, the neighborhood :math:`100` yields :math:`4`; :math:`010` yields :math:`2`, and :math:`001` yields :math:`1`. Then we can take these results and look them up in the rule table.

Here’s the more general version of ``step2``:

::

    def step3(array, i, window=[4,2,1]):
    row = array[i-1]
    c = np.correlate(row, window, mode='same')
    array[i] = table[c]

The first two lines are the same. Then the last line looks up each element from ``c`` in ``table`` and assigns the result to ``array[i]``.

Here’s the function that computes the table:

::

    def make_table(rule):
    rule = np.array([rule], dtype=np.uint8)
    table = np.unpackbits(rule)[::-1]
    return table

The parameter, ``rule``, is an integer between :math:`0` and :math:`255`. The first line puts ``rule`` into an array with a single element so we can use ``unpackbits``, which converts the rule number to its binary representation. For example, here’s the table for Rule 150:

::

    >>> table = make_table(150)
    >>> print(table)
    [0 1 1 0 1 0 0 1]

The code in this section is encapsulated in the ``Cell1D`` class, defined in ``Cell1D.py`` in the repository for this book.
