# Back-End Development Notes   

## Comments
- use comments to explain why you code something a certain way. 
 
## Main

checking if
``` 
__name__  ==  '__main__'
```
shows whether this file is being run directly by python (```name == main```) 
or is it being imported as a module (```name == file-name```)

if a file is run directly the ```__name__``` variable is set to ```main```  
importing the file as a module sets ```__name__``` to the file name

## Lambda functions

A lambda function is just like any normal python function, except that it has no name when defining it, and it is contained in one line of code.

Normal python function:

```
def a_name(x):
    return x+x
```
Lambda function:
```
lambda x: x+x
```

## Reload

To relaod your module (eg. practise module with class Dog) in the interactive interpreter:
```
from importlib import reload

import practise
reload(practise)
from practise import Dog
```

This however won't affect an instance of example_class which already exist -- they will still be of the old type Dog.

## Recursion

Met recursion een probleem oplossen kan voor memory issues zorgen omdat het vrij veel overhead heeft. Recursion is vooral handig op plekken waar subtaken snel kleiner worden (binary search) en je dus je snel kan toewerken naar basecase. 
Daardoor limiteer het aantal keer dat je de functie aanroept, < 100 fucntiecalls zou geen memory issues moeten oplever. Met een milioen functiecalls kun je beter een loop fucntie gebruiken

example: recusively flatten nested dictionaries or lists. 
example 2: recursively find the factorial of a number:

```
def iterative_factorial(n):

    if n < 1:
        return -1
    for i in range(1, n):
        n *= i
    return n
```
    

5! == 5 * 4 * 3 * 2 * 1 == 5 * 4!
```
def recursive_factorial(n):

    if n <= 1
        return -1
    # basecase
    if n == 1:
        return 1
    else:
        return = n * recursive_factorial(n - 1)
```


