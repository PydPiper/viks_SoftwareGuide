excel - Cell Split
==================
Surprisingly excel does not have a ``split`` function builtin. Ever run into a issue where
you would like to pull out a part of a result based on common delimiters? Here is a function to
solve just that.

- Function definition (this is placed in a module)

.. code-block:: vbscript

    Function xx_cellsplit(value_to_split as String, _
                          Optional ByVal delimiter As String = "/", _
                          Optional ByVal Index as Integer = 1) As String:

        ' check if input value is empty and return empty to prevent error
        If value_to_split = "" Then
            xx_cellsplit = ""
        Else
            parts = split(value_to_split, delimiter)
            xx_cellsplit = parts(index)
        End If

    End Function


- Use it in a cell

::

    =xx_cellsplit("10/20/30/40/50","/",3)
    >>> 30



