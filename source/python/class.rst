builtin - Class (Object Oriented Programming)
=============================================


Trick - Access a class's attribute by its string name
-----------------------------------------------------

.. code-block:: python
    :linenos:

    class A():
        self.attr1 = []

    getattr(A,'attr1')


Trick - Class method return a new initial definition of the same class
----------------------------------------------------------------------
This is really useful when a class __init__ is setup to take a single value input (like an ID, but instead a
range of IDs were given) and we would like to create multiple unique classes out of each ID separately.

.. code-block:: python
    :linenos:

    # take a class for instance that is a storage of attributes
    # its unique identifier is set by an attribute ID, but
    # a user would like to define multiple classes at the same time - what do we do

    class Signal():

        # note __init__ is called after __new__ via super
        def __init__(self, ID, A, B, C):
            print("initialized Signal")
            self.ID = ID
            self.A = A
            self.B = B
            self.C = C

        # called before __init__
        def __new__(cls, ID, *args, **kwargs):
            # check if ID entered was a range, if so, split them apart
            if type(ID) is list:
                print("muti-ID identified")
                return cls.split_IDs(ID, *args, **kwargs)
            else:
                # this says: from the class Signal create an instance (ie: call __init__)
                print("creating instance ID = ", ID)
                # note that .__new__(cls) only has cls as input, ID, A, B, C are not entered
                # (but they are buffered over to the __init__ automatically
                return super(Signal, cls).__new__(cls)

        @classmethod
        def split_IDs(cls, ID, *args, **kwargs):
            # return a list of Singal instances all with the same attributes A,B,C but unique single IDs
            print("creating a list of unique Signal instances")
            # note that each cls call here for each uniqueID in ID calls __new__ with ID=uniqueID as input
            # therefore this call goes to the "creating instance" logic
            return [cls(uniqueID, *args, **kwargs) for uniqueID in ID]

    # now let's test it for a single ID input:
    single_signal = Signal(ID=1,A=10,B=20,C=30)
    >>> "creating instance ID = 1"
    >>> "initialized Signal"

    # now for multi-ID input
    list_signal = Signal(ID=[1,2],A=10,B=20,C=30)
    >>> "muti-ID identified"
    >>> "creating a list of unique Signal instances"
    >>> "creating instance ID = 1"
    >>> "initialized Signal"
    >>> "creating instance ID = 2"
    >>> "initialized Signal"
