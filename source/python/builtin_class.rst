builtin - Class (Object Oriented Programming)
=============================================

Class object versus Class object instance
-----------------------------------------

- define a class

.. code-block:: python

    class Circle():
        # to set initialization parameters
        #  (these are unique attributes to each instance of the Circle class)
        def __init__(self, radius):
            # self.radius is called a INSTANCE ATTRIBUTE
            self.radius = radius
            # WHAT IS "SELF": think of self as the instance of the class; ex:
            #   at runtime: circle1 = Circle(5), the class instance "circle1" is self
            #   this is why we are able to call circle1.radius which is
            #   analogous to self.radius of that class instance


- # common mistake #1: AttributeError: type object 'Circle' has no attribute 'radius'

.. code-block:: python

    Circle.radius
    >>> AttributeError: type object 'Circle' has no attribute 'radius'
    # this occurs because we did not initialize an instance of Circle

    # fix:
    Circle(radius=10).radius
    >>> 10

- we can also save an INSTANCE of Circle as a variable (how its commonly used)

.. code-block:: python

    circle1 = Circle(10)
    circle2 = Circle(20)
    circle1.radius
    >>> 10
    circle2.radius
    >>> 20


Class Method (same as a function)
---------------------------------

- define a class with a METHOD

.. code-block:: python

    class Circle():
        def __init__(self, radius):
            self.radius = radius

        # to define a METHOD (unique name to classes, same concept as a function)
        def area(self):
            return 3.14 * self.radius ** 2

- common mistake #2: TypeError: area() missing 1 required positional argument: 'self'

.. code-block:: python

    Circle.area()
    >>> TypeError: area() missing 1 required positional argument: 'self'
    # same as above, this occurs because we did not initialize an instance of Circle

    # fix:
    Circle(radius=10).area()
    >>> 314.0

- assigned to a variable

.. code-block:: python

    circle1 = Circle(10)
    circle1.area()
    >>> 314.0

Class Attribute vs. Instance Attribute
--------------------------------------

- define a class with CLASS and INSTANCE ATTRIBUTES

.. code-block:: python

    # classes are great with their simple dot completion attributes, however
    #  results can be very different than expected when using different attribute types

    class Circle():
        # CLASS ATTRIBUTE: same for all instances of the class
        PI = 3.14           # immutable CLASS ATTRIBUTE
        classlist = [1,]    # mutable CLASS ATTRIBUTE

        def __init__(self, radius):
            # INSTANCE ATTRIBUTE: unique to each instance of the class
            self.radius = radius

- define 2 instances of the parent class Circle

.. code-block:: python

    # lets create 2 instances of the class Circle
    circle1 = Circle(radius=5)
    circle2 = Circle(radius=10)

    # note that circle1 and 2 both have a CLASS ATTRIBUTE .PI that is the same
    circle1.PI
    >>> 3.14
    circle2.PI
    >>> 3.14
    # but their INSTANCE ATTRIBUTE is unique to each instance of the class Circle
    circle1.radius
    >>> 5
    circle2.radius
    >>> 10

- Updating CLASS ATTRIBUTES

.. code-block:: python

    # CLASS ATTRIBUTES are connected to all instances of that class,
    #   we can change all of them at once by modifying the master CLASS ATTRIBUTE
    circle1.PI
    >>> 3.14
    circle2.PI
    >>> 3.14
    # now lets update both from the parent class Circle
    Circle.PI = 50
    circle1.PI
    >>> 50
    circle2.PI
    >>> 50

- Updating CLASS ATTRIBUTES the wrong way!

.. code-block:: python

    # IMPORTANT: python lets you do whatever you like, but with such power comes consequences
    #   ex: the ability to overwrite a CLASS ATTRIBUTE of a class instance like circle1
    #   note that prior to modifying .PI CLASS ATTRIBUTE has the same ID for all instances
    id(circle1.PI)
    >>> 72539584
    id(circle2.PI)
    >>> 72539584
    # now when we overwrite .PI we are actually changing the .PI attribute from CLASS to INSTANCE ATTRIBUTE
    circle1.PI = 3
    id(circle1.PI)
    >>> 1865210064
    # also note that now instances DO NOT share the same .PI CLASS ATTRIBUTE any more
    circle2.PI
    >>> 3.14


    # now lets see what happens with a mutable CLASS ATTRIBUTE
    id(circle1.classlist)
    >>> 71716696
    id(circle2.classlist)
    >>> 71716696
    # similar to PI, classlist shares the same ID between classes, but now updating one
    #   also updates all because the ID stays the same for mutable objects
    circle1.classlist += [2]
    circle1.classlist
    >>> [1,2]
    circle2.classlist
    >>> [1,2]       # circle2 instance was also updated!


Class Methods (method, staticmethod, classmethod)
-------------------------------------------------

- define a class with a METHOD, STATICMETHOD, and CLASSMETHOD

.. code-block:: python

    # class methods are analogous to function definitions, except they are tied to a class
    class Circle():
        def __init__(self, radius):
            self.radius = radius

        # This is a simple METHOD: methods take at least 1 argument "self" and does something with it
        def area(self):
            return 3.14 * self.radius ** 2

        # This is a STATICMETHOD: a static method does not depend on "self"
        #   or more explicitly stating, any unique definition of the class instance
        @staticmethod
        def color(color='black'):
            return 'the color of the circle is: ' + color

        # This is a CLASSMETHOD: a class method takes at least 1 argument "cls" and
        #   it usually returns a new altered instance of the class
        # What is really special about a class method is that the
        #   user is able to call it without instancing the class (see example below)
        @classmethod
        def from_dia(cls, diameter):
            # cls under the hood calls Circle.__new__() that creates a new instance of the class Circle
            # with new __init__ definition that is: diameter/2
            return cls(diameter / 2)

- Call/use a METHOD

.. code-block:: python

        circle1 = Circle(radius=5)
        # call a regular METHOD via
        circle1.area()
        >>> 78.5

- Call/use a STATICMETHOD

.. code-block:: python

        circle1 = Circle(radius=5)
        # call a STATICMETHOD
        circle1.color()
        >>> 'the color of the circle is: black'

- Call/use a CLASSMETHOD. Define a Circle by diameter (note that the class is never instanced, ie: "Circle()")
  circle2 is now instanced via CLASSMETHOD, and all of the regular functionality is available

.. code-block:: python

        circle2 = Circle.from_dia(diameter=10)
        circle2.radius
        >>> 5.0
        circle2.area()
        >>> 78.5


Double underscore methods (dunder)
----------------------------------

- define a class with ``__init__``, ``__repr__``, ``__call__``

.. code-block:: python

    class Circle():
        # INIT: initialize a class instance with parameters
        def __init__(self, radius):
            self.radius = radius

        # REPR: string representation of a class (instead of the default "Circle object at 0x23423423"
        def __repr__(self):
            return "Circle Class"

        # CALL: returns call to the class instance
        def __call__(self, *args, **kwargs):
            print(args)
            args = args if args else ("",)
            print(args)
            return "this is a call on the class, " + len(args)*"{},".format(*args)

        # ADD: defines what to do with a "+" operator
        #  note: operators always work from left, ie: Circle + 10
        #        the "+" operator is actually calling __add__ on Circle
        def __add__(self, arg):
            print("you tried to add to class Circle")
            return arg + self.radius

        def __subtract__(self, arg):
            return "you tried to subtract from class Circle"

        def __mul__(self, arg):
            return "you tried to multiply class Circle"

        def __truediv__(self, arg):
            return "you tried to divide class Circle"

        # you can have the operator read from the right as well, this is useful if you
        #  tried to add: 10 + Circle, by default python will try to read from left but
        #  has no idea how to add a "int" + "class" so then it will look to the right and
        #  see if it has a "radd" definition, the "r" can be defined for all other math operators
        def __radd__(self, arg):
            print("addition with right operator")
            return arg + self.radius

        # to evaluate Circle[arg] sequence
        def __getitem__(self, arg):
            return [self.radius]


- call/use ``__init__`` (class instance initialization)

.. code-block:: python

    # INIT call/use
    circle1 = Circle(radius=5)

- call/use ``__repr__`` (class text representation)

.. code-block:: python

    circle1 = Circle(radius=5)
    # REPR call/use
    circle1
    >>> "Circle Class"
    # REPR call/use
    str(circle1)
    >>> "Circle Class"

- call/use ``__call__`` (call return of the class)

.. code-block:: python

    circle1 = Circle(radius=5)
    # CALL call/use
    circle1()
    >>> "this is a call on the class, ,"
    circle1(1,2)
    >>> "this is a call on the class, 1,2"

- call/use ``__add__`` and other math dunder's

.. code-block:: python

    circle1 = Circle(radius=5)
    # ADD call/use
    circle1 + 5 # here __add__ gets called
    >>> "you tried to add to class Circle"
    >>> 10 # radius + 5

    # now a right operation, since int doesnt know how to add Circle, but Circle does
    5 + circle1 # int + Circle returns an error, then python tried from right: __radd__ gets called
    >>> "addition with right operator"
    >>> 10

Subclassing - to extend functionality of a class
-------------------------------------------------
Take Circle class for instance, it has a method to calculate area
now lets say Circle is locked down as a class by another coder and we cannot change it
we dont want to start from scratch and rebuild Circle, but we do want to add functionality
we can do this with subclassing

- define a parent class and a subclass (a subclass inherits functionality of a parent class)

.. code-block:: python

    # here is the original Circle Class
    class Circle():
        def __init__(self, radius):
            self.radius = radius

        def area(self):
            return 3.14 * self.radius ** 2


    # now lets create a custom Class that inherits functionality from Circle
    class CustomCircle(Circle):
        def halfarea(self):
            # note that we depend on Circle having a method called area()
            #   but the method itself is not defined here in CustomCircle
            #   it is INHERITED
            return self.area() / 2

- using a subclass

.. code-block:: python

    # lets create an instance of our custom class
    circle1 = CustomCircle(radius=10)
    # note that we still have access to methods from Circle (it is INHERITED)
    circle1.area()
    >>> 314.0
    # but we also have a new custom functions from CustomCircle
    circle1.halfarea()
    >>> 157.0


Trick - Print the docstring of a class/method
---------------------------------------------

.. code-block:: python

    class Circle():
        """
        Class docs
        """

        def __init__(self):
            """
            Instance docs
            """
            pass

        def func(self):
            """
            Method docs
            """
            pass

    Circle.__doc__
    >>> "Class docs"
    Circle.__init__.__doc__
    >>> "Instance docs"
    Circle.func.__doc__
    >>> "Method docs"


Trick - Testing that a class has a method (compile time)
--------------------------------------------------------

.. code-block:: python

    assert hasattr(Circle, "area"), "The class Circle doesnt have required method area"


Trick - Access a class's attribute by its string name
-----------------------------------------------------

.. code-block:: python

    class A():
        self.attr1 = []

    getattr(A,'attr1')


Trick - How to create multiple levels of attributes
---------------------------------------------------------
There are a lot of lessons on object oriented programming that talk about the big concepts,
then you go to create something in practice and realize that you are still missing some fundamentals.
For me, this was the case on nested attributes for a long time. How do I create a class with multiple
levels of attributes?

- Lets say we want have a database class (storage class), and each time is index by an ``ID``
  but under each ``ID`` we would like to call more attributes, ie: ``db.ID[1].att1``. Let's see how
  that can be done:

.. code-block:: python

    # database class
    class Database():
        def __init__(self):
            # initialize the ID container as a dictionary
            self.ID = {}

        # create a method of adding new items to the database
        def additem(self,ID,att1,att2):
            # update database dictionary by calling another class "Attributes"
            #  this says: Database.ID[#] returns the class Attributes,
            #  that can then be called for it's attributes ".att1", ".att2"
            self.ID.update({ID: Attributes(att1,att2)})


    # 2nd level nested attributes class
    class Attributes():
        def __init__(self,att1,att2):
            self.att1 = att1
            self.att2 = att2


    # lets see how it works in pratice

    # define a instance of the database
    db = Database()
    # add a few items
    db.additem(1,'att1 from ID1', 'att2 from ID1')
    db.additem(2,'att1 from ID2', 'att2 from ID2')
    # now to call it nested
    db.ID[1].att1
    >>> 'att1 from ID1'
    db.ID[2].att2
    >>> 'att2 from ID2'



Trick - Create multiple instances of a class based on initial input
-------------------------------------------------------------------
This is really useful when a class __init__ is setup to take a single value input (like an ID, but instead a
range of IDs were given) and we would like to create multiple unique classes out of each ID separately.

.. code-block:: python

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
