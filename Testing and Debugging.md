# Testing and Debugging

## Python pdb

### breakpoint()  
Insert ```breakpoint``` at the location where you want to enter the debugger. Will import pdb and call pdb.set_trace() from Pyhton 3.7 so that you won't need ```import pdb; pdb.set_trace()```.
When the breakpoint line is executed, Python pauses in the interactive debugger and and waits for a command.

### **p**  - print  
Prints expressions. Can be used to print values of variables, functions, paths, filenames, dictionaries etc. You can also use the pretty print command pp
### **s** - step 
executes the current line and stops in, or 'steps into' a foreign function if one is called. If execution is stopped in another function, s will print *--Call--*. 
### **n** - next  
Continues execution until the next line and stay within the current function, i.e. not stop in a foreign function if one is called (like with **s**).
Both s and n will stop execution when the end of the current function is reached and print  *--Return--* along with the return value at the end of the next line after ->
### **c** continue
Continue execution and only stop when a breakpoint is encountered.
### **r**  return
Can be used to exit out of a function. ```return``` runs execution to the end of the current function and then stops. Enter n(ext) once to complete the step out. 
### **q** quit 
Quits the debugger and exit pdb


[1. Phyton Debuggging with Pdb](https://realpython.com/python-debugging-pdb/)