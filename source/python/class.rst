builtin - Class (Object Oriented Programming)
=============================================

General
-------

- Class object versus Class object instance

.. code-block:: python

    # to define a class
    class Circle():
        # to set initialization parameters (these are unique attributes to each instance of the Circle class)
        def __init__(self, radius):
            # self.radius is called a INSTANCE ATTRIBUTE
            # WHAT IS "SELF": think of self as the instance of the class; ex:
            # at runtime: circle1 = Circle(5), the class instance "circle1" is self
            # this is why we are able to call circle1.radius which is analogous to self.radius of that class instance
            self.radius = radius
        # to define a METHOD (unique name to classes, same concept as a function)
        def area(self):
            return 3.14 * self.radius ** 2

    # common mistake #1: AttributeError: type object 'Circle' has no attribute 'radius'
    Circle.radius
    >>> AttributeError: type object 'Circle' has no attribute 'radius'
    # this occurs because we did not initialize an instance of Circle, fix:
    Circle(radius=10).radius
    >>> 10
    # we can also save an instance of Circle as a variable (how its commonly used)
    circle1 = Circle(10)
    circle2 = Circle(20)
    circle1.radius
    >>> 10
    circle2.radius
    >>> 20


    # common mistake #2: TypeError: area() missing 1 required positional argument: 'self'
    Circle.area()
    >>> TypeError: area() missing 1 required positional argument: 'self'
    # same as above, this occurs because we did not initialize an instance of Circle, fix:
    Circle(radius=10).area()
    >>> 314.0
    # assigned to a variable:
    circle1 = Circle(10)
    circle1.area()
    >>> 314.0

- Class Attribute vs. Instance Attribute

.. code-block:: python
    :linenos:

    # class are great with their simple dot completion attributes,
    # results can be very different than expected depending on the attribute type

    class Circle():
        # CLASS ATTRIBUTE: same for all instances of the class
        PI = 3.14
        classlist = [1,]

        def __init__(self, radius):
            # INSTANCE ATTRIBUTE: unique to each instance of the class
            self.radius = radius

    circle1 = Circle(5)
    circle2 = Circle(10)

    # note that circle1 and 2 both have a CLASS ATTRIBUTE .PI that is the same but their INSTANCE ATTRIBUTE is unique
    circle1.PI
    >>> 3.14
    circle1.radius
    >>> 5
    circle2.PI
    >>> 3.14
    circle2.radius
    >>> 10

    # although you are able redefine CLASS ATTRIBUTES on runtime.. DONT! Kittens will die!
    # CLASS ATTRIBUTES are meant to be the same for all instances of a class
    # and changing them at runtime does not propagate to other instances of the class:
    # (this is true for all immutable CLASS ATTRIBUTES)
    id(circle1.PI)
    >>> 72539584
    id(circle2.PI)
    >>> 72539584
    # now note that changing it on runtime does not update the CLASS ATTRIBUTE,
    # instead it overwrite it to be INSTANCE ATTRIBUTE
    circle1.PI = 3
    id(circle1.PI)
    >>> 1865210064
    # now note circle2 is still unchanged, the change did not propagate
    circle2.PI
    >>> 3.14

    # now lets see what happens with a mutable CLASS ATTRIBUTE
    id(circle1.classlist)
    >>> 71716696
    id(circle2.classlist)
    >>> 71716696
    # similar to PI, classlist shares the same ID between classes, but now updating one also updates all
    # because the ID stays the same for mutable objects
    circle1.classlist += [2]
    circle1.classlist
    >>> [1,2]
    circle2.classlist
    >>> [1,2] # circle2 instance was also updated!


- Class Methods (method, staticmethod, classmethod)

.. code-block:: python
    :linenos:

    # class methods are analogous to function definitions, except they are tied to a class
    class Circle():
        def __init__(self, radius):
            self.radius = radius

        # This is a simple METHOD: methods take at least 1 argument "self" and does something with it
        def area(self):
            return 3.14 * self.radius ** 2

        # This is a STATICMETHOD: a static method does not depend on "self"...
        # or more explicitly stating, any unique definition of the class instance)
        @staticmethod
        def color(color='black'):
            return 'the color of the circle is: ' + color

        # This is a CLASSMETHOD: a class method takes at least 1 argument "cls" and...
        # it usually returns a new altered instance of the class
        # What is really special about a class method is that the...
        # user is able to call it without instancing the class (see example below)
        @classmethod
        def from_dia(cls, diameter):
            # cls under the hood actually calls Circle.__new__() that creates a new instance of the class Circle
            # with new __init__ definition that is: diameter/2
            return cls(diameter / 2)

        circle1 = Circle(radius=5)
        # call a regular METHOD via
        circle1.area()
        >>> 78.5
        # call a STATICMETHOD
        circle1.color()
        >>> 'the color of the circle is: black'

        # define a Circle by diameter (note that the class is never instanced, ie: "Circle()")
        circle2 = Circle.from_dia(diameter=10)
        # circle2 is now a instanced via CLASSMETHOD, and all of the regular functionality is avaliable
        circle2.radius
        >>> 5.0
        circle.area()
        >>> 78.5


- double underscore methods (dunder)

.. code-block:: python
    :linenos:

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

    # INIT call/use
    circle1 = Circle(radius=5)
    # REPR call/use
    circle1
    >>> "Circle Class"
    # REPR call/use
    str(circle1)
    >>> "Circle Class"
    # CALL call/use
    circle()
    >>> "this is a call on the class, ,"
    circle(1,2)
    >>> "this is a call on the class, 1,2"

- Subclass to extend functionality of a class

.. code-block:: python
    :linenos:

    # take Circle class for instance, it has a method to calculate area
    # now lets say Circle is locked down as a class by another coder and we cannot change it
    # we dont want to start from scratch and rebuild Circle, but we do want to add functionality
    # we can do this with subclassing

    # here is the original Circle Class
    class Circle():
        def __init__(self, radius):
            self.radius = radius

        # This is a simple METHOD: methods take at least 1 argument "self" and does something with it
        def area(self):
            return 3.14 * self.radius ** 2

    # now lets create a custom Class that inherits functionality from Circle
    class CustomCircle(Circle):
        def halfarea(self):
            # note that we depend on Circle having a method called area()
            # but the method itself is not defined here in CustomCircle
            # it is inherited
            return self.area() / 2

    # to call create it and use it:
    circle1 = CustomCircle(radius=10)
    # we still have access to methods from Circle
    circle1.area()
    >>> 314.0
    # but we also have new custom functions from CustomCircle
    circle1.halfarea()
    >>> 157.0



Trick - Access a class's attribute by its string name
-----------------------------------------------------

.. code-block:: python
    :linenos:

    class A():
        self.attr1 = []

    getattr(A,'attr1')


Trick - Create multiple instances of a class based on initial input
-------------------------------------------------------------------
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
