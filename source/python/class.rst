builtin - Class (Object Oriented Programming)
=============================================


Trick - Access a class's attribute by its string name
-----------------------------------------------------

.. code-block:: python
    :linenos:

    class A():
        self.attr1 = []

    getattr(A,'attr1')
