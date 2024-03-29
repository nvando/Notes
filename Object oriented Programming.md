# Object Oriented Programming
Object-oriented Programming (OOP) is a programming model in which programs are structured so that related properties and behaviors are bundled into individual objects. It organizes software design around data, or objects, rather than functions and logic.  

The name of a clase is defined with camelcase

By OOP de data zelf en de functies die met de data werken (wat je met die data kan doen) zijn samen gegroepeerd in een class. Met OOP kun je data beter beschermen omdat je de data alleen kan aanpassen met een gemiliteerde set functies van de class, en je hebt dus volledige controle over hoe de data veranderd wordt (encapsulatie).


## Classes
A class is a blueprinth for creating objects (object instances) and defines the properties (attributes and behaviour (methods) associated with it.

An object is an instance of class (eg An Audi is an instance of the class Car)

## Class Objects and Instances  
While the class is the blueprint, an instance is a specific object that is built from a class and contains real data. 

For instance: an instance of the Dog class is not a blueprint anymore. It’s an actual dog with a name, like Miles, who’s four years old.

*A class is like a form or questionnaire. An instance is like a form that has been filled out with information. Just like many people can fill out the same form with their own unique information, many instances can be created from a single class.*

A new onject is instantiated by typing the name of the class, followed by opening and closing parentheses:
```
>>> Dog()
<__main__.Dog object at 0x106702d30>
```
Above code creates a new Dog object at 0x106702d30. The string of letters and numbers is a memory address that indicates where the Dog object is stored in your computer’s memory. 

```d = dog()``` assigns a variable d to the instance of class dog (*instanciating*: creating a new instance)


### Difference between Class >> Object >> Instance

*Class* = A blue print (ig for a housedesign).   
*Object* = an actual thing that is built based on the ‘blue print’ (ig. all the houses built from above blueprint are objects of that class).   
*Instance* = is a virtual copy of the object (ig a goven or specific house )



## Methods
A method is a function that goes inside of a class. Methods identify the behaviors and actions that an object created from the class can perform with its data: they are used to define the behaviors of an object.  

Methods are a reusable piece of code that can be invoked/called at any point in the program.  

*Instance methods*
Instance methods: Used to access or modify the object state: they can access the unique data of their instance.

For instance, if you have two objects each created from the class car, then they each may have different properties such as colors, engine sizes, seats, and so on.

Any method you create will automatically be created as an instance method, so no decorator is needed

*Class method vs Static Method*  
Both a class method and a static method are bound to the class and not the object of the class. A class method however, can access or modify the class state while a static method can’t access or modify it. Use @classmethod decorator or @staticmethod decorator to create the specific method in python.

*dir() method*
Use the dir() on the class to get a list of all attributes and methods, including the dunder methods


## Calling a method
Het is gebruikelijk om de methode aan te roepen op de instance ipv op de class en de instance mee te geven, dus zo

```
>>> miles.speak()
'miles says woof'
```

in plaats van zo:
```
>>> Dog.speak(miles)
'miles says woof'
```

Whenever we call a method on an instance, Python looks on the class for our function and then passes the instance and all other arguments in

When we made an instance of our class and then called a method on this instance:
```
>>> door = Thing("red", 4)
>>> door.paint_it_black()
```
Python translated this to:

```
>>> Thing.paint_it_black(door)
```
So tehse two lines of code are the same:

```
>>> Thing.paint_it_black(door)
>>> door.paint_it_black()
```
This principle applies to all classes.

Given a string and a list:
```
>>> greeting = "hello"
>>> numbers = [1, 2, 3]
```
Calling a method on either of these objects is the same as calling the method on the class and passing the class instance in.

So these two lines are equivalent:

```
>>> greeting.upper()
'HELLO'
>>> str.upper(greeting)
'HELLO'
````
As are these two:
```
>>> numbers.append(4)
>>> list.append(numbers, 4)
```
You shouldn't call MyClass.my_method(my_instance) as that's Python's job. The way we as Python programmers call methods is my_instance.my_method(). But under the hood Python translates that to type(my_instance).my_method(my_instance).


## Dunder or magic methods

Dunder (double underscore) or magic methods are not meant to be invoked directly by you, but the invocation happens internally from the class on a certain action. For example, when you add two numbers using the + operator, internally, the ```__add__()``` method will be called.

```
>>> num = 10
>>> num + 5
15
>>> num.__add__(5)
15
```
When you do num + 5, the + operator calls the ```__add__(5)``` method. You can also call ```num.__add__(5)``` directly which will give the same result.

### The ```__init__``` method

The ```__init__ ```method is invoked without any call, whenever an new instance of an object is created: it is the initializer method that is first run as soon as the object is created.  

### The ```__str__``` method
The ```__str__()``` method returns the string representation of the object. This method is called internally when the print() or str() function is invoked on an object. 

The default ```__str__()``` method of an object's class doesn't tell any meaningful info of the object other than its id.
```
>>> print(max)
>>> <__main__.myclass object at 0x000001E06B3ADE08>
```
If you want a string representation showing values of object attributes, can can override the ```__str__()``` method in the myclass, as shown below.
```
def __str__(self):
        return f"{self.name} is a dog and {self.age} years old"

>>> print(max)
>>> Max is a dog and 12 years old
```


## Attributes
The attributes are a characteristic of an object. 
   
*Class attributes* are the same for all instances of a class: the variable belongs to a class rather than a particular object. It is defined outside the constructor function, ```__init__```(self...), of the class. They must always be assigned an initial value.

*Instance attributes* are different for every instance of a class: these variables belong to one, and only one, object. An instant atrribute is only accessible in the scope of this object. These attributes are defined inside the ```__init__``` method of the class. 

```
class Dog:
    # Class attribute
    species = "Canis familiaris"

    def __init__(self, name, age):
    # Instance attributes
        self.name = name
        self.age = age
```

### Accessing class attributes:

```
class Person:
    number_of_people = 0

    def__init__(self, name):
    Person.number_of_people += 1


p1 = Person("tim")
p2 = person("jill")
```
```number_of_people``` is defined for the entire class
```
>>> print(p1.number_of_people)
>>> 0
```
Because this attribute is not specific for any instance, you can also access and change it by using the name of the class
```
>>> print(Person.number_of_people)
>>> 0
>>> Person.number_of_people = 8
>>> print (p2.number_of_people)
>>> 8
```


## Self
Self represents the instance of the class (self is simply a variable to points to the instance of the class you currently are working with). Through the “self” keyword the attributes and methods of the class can be accessed. It binds the attributes with the given arguments. 

When a new class instance is created, the instance is automatically passed to the self parameter in ```__init__```() so that new attributes can be defined on the object.

It does not have to be named self, but it has to be the first parameter of any function in the class. 

When writing a method the first argument is always ```self```, because we need to invisible pass the actual object instance (dog object), so that we know which instance we are accessing when we apply that method.  

## Inheritance

Inheritance allows you to define a class, the **child class** that inherits all the methods and properties from another class, the **parent class**.   


```
class Student(Person):
  pass
```
This student class has all the same attributes and methods as the Person class. With child classes you can not only accept the default parent methods (with the pass keyword), but also add more methods, or override existing parent methods.

You can use the super() function to call a parent method into a child method to make use of it. For example, we may want to override one aspect of the parent method with certain functionality, but then call the rest of the original parent method to finish the method.

```
class Student(Person):
  def __init__(self, fname, lname, year):
    super().__init__(fname, lname)
    self.graduationyear = year
```
Use ```super()__init__()``` instead of ```Person___init___() ``` because: 
1. it allows us to avoid using the base class name explicitly 
2. it also works with Multiple Inheritance.

Since we do not need to specify the name of the base class when we call its members, we can easily change the base class name (if we need to).





### References

[1. Python OOP tutorial on youtube - Tech With Tim](https://www.youtube.com/watch?v=JeznW_7DlB0)  
[2. OOP at Programiz.com](https://www.programiz.com/python-programming/object-oriented-programming)   
[3. Inheritance ](https://www.digitalocean.com/community/tutorials/understanding-class-inheritance-in-python-3)   
[4. Python Super()](https://www.programiz.com/python-programming/methods/built-in/super)  
[5. Sefan Wijnja - Classes 2021-03-10](https://vimeo.com/521969539/642927e69d)



