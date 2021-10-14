# Object Oriented Programming
Object-oriented Programming (OOP) is a programming model in which programs are structured so that related properties and behaviors are bundled into individual objects. It organizes software design around data, or objects, rather than functions and logic.  

The name of a clase is defined with camelcase
` `  
` `  
` `  
### Objects
A class is a blueprinth for creating objects (object instances) and defines the properties (attributes and behaviour (methods) associated with it.

An object is an instance of class (eg An Audi is an instance of the class Car)
` `  
` `  
` `  
### Methods
A method is a function that goes inside of a class. Methods identify the behaviors and actions that an object created from the class can perform with its data: they are used to define the behaviors of an object.  

Methods are a reusable piece of code that can be invoked/called at any point in the program.  
` `  
` `  
### Instances  
An instance is a specific object that is built from a particular class and contains real data.  

```d = dog()``` assigns a variable d to the instane of class dog (*instanciating*: creating a new instance)

The ```__init__ ```method will be called whenever an new instance of an object is created: it is the initializer method that is first run as soon as the object is created.  
` `  
` `  
` `
### Attributes
The attributes are a characteristic of an object. 
   
*Class attributes* are the same for all instances of a class.   

*Instance attributes* are different for every instance of a class. These attributes are defined inside the ```__init__``` method of the class. 

` `  
` `  

### Self
Self represents the instance of the class (self is simply a variable to points to the instance of the class you currently are working with). Through the “self” keyword the attributes and methods of the class can be accessed. It binds the attributes with the given arguments.

It does not have to be named self, but it has to be the first parameter of any function in the class.

When writing a method the first argument is always ```self```, because we need to invisible pass the actual object instance (dog object), so that we know which instance we are accessing when we apply that method.  
` `  
` `  
### References

[1. Python OOP tutorial on youtube - Tech With Tim](https://www.youtube.com/watch?v=JeznW_7DlB0)  
[2. OOP at Programiz.com](https://www.programiz.com/python-programming/object-oriented-programming)


