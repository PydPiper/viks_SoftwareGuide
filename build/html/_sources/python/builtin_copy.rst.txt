builtin - Copy True/Shallow/Deep
================================
There are 3 different ways to copy objects in python:

- **True Copy**: ID or old object and new object are the **same**, therefore changing one object
  also changes the other

- **Shallow Copy**: ID of old object and new object is **different**, therefore the number of contents
  and immutable contents can be changed by one that will not effect the other, HOWEVER mutable content copied
  are still linked between the two objects (ie: a = [[1,2],3], copy_a = [[1,2],3] the list [1,2] inside is the
  same ID, therefore changing the list in "a" will also change the list in "copy_a"

- **Deep Copy**: creates **new** IDs for each object within (very slow!). There is no link between old and new copy

For more description on mutable/immutable see :doc:`builtin_mutable`

For examples on copy see :ref:`list_copy`
