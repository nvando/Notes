# Paths and the Pathlib module

## Absolute or Relative paths
*absolute path*: begins from the root directory, specifies the complete directory tree, 
*relative path*: is the path of a file relative to another file/directory (usually the current directory)    
` `  
` `   
## Why use the Pathlib module:

- combines the best of file system  modules such as os, os.path, glob, etc.
- more readable and easier to use
- builds up paths by representing them as proper objects, instead of raw strings
- code is portable across platforms
- deals with absolute and relative paths.  
` `  
` `  
## What does Pathlib do 
 The pathlib module deals with path related tasks by:
- constructing new paths from filenames and other paths
- checking properties of paths 
- creating files/folders at specific paths.  
` `  
` `  
## How to use Pathlib

either import all classes from module [```from pathlib import *```]  
or import module as a whole [```import pathlib```]

when importing the whole module,   
all subsequent uses of classes should be prefixed with the module name:
```
current_dir = pathlib.Path.cwd()
home_dir = pathlib.Path.home()
```

use ```Path.cwd()``` instead of ```os.getcwd()```  
use  ```/``` operator instead of ```os.path.join``` 
