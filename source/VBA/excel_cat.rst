excel - Concatenate Multiple
============================
Excel does have a ``CONCATENATE`` function builtin, however it does not let you select a range,
which can be particularly frustrating when trying to concatenate a handful of cells. Not to mention,
if we would like to place delimiters in between the concatenation, we would have to type ``& "/" &``
between each cat. Here is a much more user friendly version of concatenation:

.. code-block:: vbscript

    Function xx_cat(ref as Range, Optional ByVal delimiter as String = "") as String

        Dim cell as Range
        Dim result as String

        ' step through each cell and concatenate the results if the cell is not empty
        For Each cell in ref
            if IsEmpty(cell) = False Then
                result = result & cell.value & delimiter
            End If
        Next cell

        ' return the results without the last delimiter
        xx_cat = Left(result, len(result) - 1)

    End Function

- Use it in a cell, where cell

    - A1=1
    - A2=2
    - A3=3
    - A4=4
    - A5=5

::

    =xx_cat(A1:A5,"/")
    >>> 1/2/3/4/5

