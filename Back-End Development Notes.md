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

if a file is run directly the ```__name__``` variableis set to ```main```  
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
