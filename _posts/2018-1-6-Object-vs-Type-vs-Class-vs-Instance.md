---
layout: post
title: Object vs Type vs Class vs Instance
---

Description of the relationship between objects, types, classes, and instances in Python.

## Objects

Objects are a way of representing any entities in Python. All data in Python is represented by objects and relationships between objects.
Each object has an identity, type, and value. The identity never changes from the moment the object is created.
The `is` operator compares the identities of two objects; the `id()` function returns the integer identifier of an object.

*For CPython, id(x) is the memory address where x is stored.*

```
>>> a = 127
>>> b = 128
>>> id(a)
1465020576
>>> id(b)
1465020608
>>> a is b
False
```

In Python, everything is an object. Lists are objects. Strings, modules, and functions are objects. The bytecode of a program is stored as an object. All objects have types and identities:

```
>>> def dummy(): pass
...
>>> type(dummy), id(dummy)
(<class 'function'>, 2122090425880)
>>> type(dummy.__code__), id(dummy.__code__)
(<class 'code'>, 2122094942368)
>>>
```

The "everything is an object" model is followed in the CPython implementation. In the CPython code, any entity mentioned above can be accessed through a pointer to the PyObject structure.

## Types
Every object in Python has a type. You can find out the type of an object by calling the built-in function `type()`. The type is also an object, so it has its own type, which is called `type`. This fact is not very useful for writing simple Python code, but it is important for understanding the internals of CPython:

```
>>> type(42)
<class 'int'>
>>> type(type(42))
<class 'type'>
>>> type(type(type(42)))
<class 'type'>
```
All types converge to type.

## Classes
Long ago, there was a difference between built-in classes and user-defined classes, but with version 2.2 (new-style classes in 2.x and all classes in 3.x), there are no significant differences. In essence, a class is a mechanism in Python that allows you to create custom types in Python code.
```
>>> class Snake: pass
...
>>> s = Snake()
>>> type(s)
<class '__main__.Snake'>
>>>
```
Using this mechanism, we created Snake, a user-defined type. s is an instance of the Snake class. Or, in other words, s is an object, and its type is Snake. The terms "class" and "type" both refer to the same concept.

## Instances (instance)
Unlike the ambiguity of the terms "class" and "type", "instance" is synonymous with the term "object".
Again: objects are instances of types. So, "42 is an instance of the int type" is equivalent to "42 is an int object".
The term "instance" is used in the built-in function `isinstance()`.

In Python, everything is an object, so any user-defined class is also an object, and its type *usually* is "type".
Again: all user-defined types *usually* are instances of the type class.

For example, the Snake class:
```
>>> type(Snake)
<class 'type'>
```

We introduce a new term: Metaclass - this is a special class whose constructor returns a class. That is, the relationship metaclass->class is analogous to the relationship class->object. The fact that classes are instances of the "type" class allows us to create our own metaclasses (classes that inherit from the "type" class and whose constructor returns a class).

```
>>> class TestMeta(type): pass  # create the simplest metaclass
...
>>> MyClass = TestMeta("myclass",tuple(),{})  # create a metaclass instance
>>> MyClass
<class '__main__.myclass'>
>>> type(MyClass)  # got a user-defined class with a type different from type
<class '__main__.TestMeta'>
>>> type(type(MyClass))
<class 'type'>
```

Why? Typically, metaclasses are used when writing complex frameworks at the level of Django ORM, they allow you to move part of the internal magic above the user code, so that "user" does not delve into the implementation details. As the guru of Python, Tim Peters, said, *If you're unsure whether you need them - you don't.*

## Conclusion
The naive question arises: so what's more important - type or object?
```
>>> type.__bases__
(<class 'object'>,)
>>> object.__bases__
()
```
type inherits from object, but also:
```
>>> type(object)
<class 'type'>
```
object has type type. In essence, object is the root of the object hierarchy, and type is the root of the type hierarchy. Each type is an object, and each object has a type, which is why there is such duality in their definitions. There is no original source among them; this is the only similar exception built into the language.

Consider the standard classes. The bool class inherits from the int class. On the diagram, object inheritance is shown in green, type inheritance is shown in red.


![_config.yml]({{ site.baseurl }}/images/Diagram1.png)


And add the classes from the examples above:


![_config.yml]({{ site.baseurl }}/images/Diagram2.png)


Thus, metaclasses allow you to implement a type hierarchy similar to object inheritance. The practical significance of metaclasses is that when creating a class, we can execute arbitrary code that modifies the created class without having to make changes to the class itself *(but in 99% of cases, such a thing is better done through class decorators/inheritance)*.


## Note
It is important to distinguish between the built-in function `isinstance()` and the operator `is`:

`isinstance(object, class)` returns True if `object` is an instance of `class`, or an instance of a class inherited from `class`.

The operator `is`, in turn, returns True if two variables refer to the same object, i.e., it compares the identities of objects.

## Dynamic Class Creation via type()
Instead of calling with one argument, `type()` can be called with three:

`type(classname, superclasses, attributes_dict)`

If `type` is called with three arguments, it will return a new object `type`. This allows you to dynamically form classes (as done in the metaclass example):

* "classname" - a string that defines the name of the class and becomes its attribute `__name__`;
* "superclasses" - a tuple of parent classes, which becomes the attribute `__bases__`;
* "attributes_dict" - a dictionary representing the namespace of our class. It contains the definition of the class body and becomes its attribute `__dict__`. The attributes of a class can be any objects:

```
>>> m = type("mytype", (int,), {"test": "field", "greet": lambda: print("hello word")})
>>> m.test
'field'
>>> m.greet()
hello word
```

Sometimes type() is called a built-in function, but you need to understand that type(1), although it looks like print(1), is actually a call to the type constructor with one argument. Calling the constructor with one argument returns the type of the passed object, with three - creates a new type. Here is the signature of `type.__init__()`:
```
def __init__(cls, what, bases=None, dict=None):
        """
        type(object_or_name, bases, dict)
        type(object) -> the object's type
        type(name, bases, dict) -> a new type
        """
        ...
```

