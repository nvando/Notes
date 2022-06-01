# Virtual Environments
*From online class 2021-09-21*
` `  
` `  
**virtual environment**:  an isolated environment for a python project


- It isolates the Python interpreter, libraries and scripts installed in it, from those installed elsewhere
- It helps resolve dependency issues by allowing the use of different versions of a package for different projects.  
` `  
` ` 
` `  
## Creating a virtual env  
` `  
` `  
1. Creating the virtual env. 'env' will be the folder name which holds the virt env  
    ```
    python3 -m venv env 
    ```
` `  

2. Activating the virt env by reading the activation script 
` `  

    On Windows, run:
    ```
    tutorial-env\Scripts\activate
    ```
    On Unix or MacOS, run:
    
    ```
    source env/bin/activate
    ```

` ` 

3. Retrieving the requirements of the project and installs them 
    ```
    pip install -r requirements.txt
    ```
` `
 
4. Getting out of the virt env  
    ```
    deactive
    ````
    ` `  
    ` `  

## Creating a requirements file

Met requirements.txt kun je je virt env inrichten zoals je wil, dus met alle dependencies en juiste versies.

Het vastzetten van versies zorgt ervoor dat je op verschillende locaties dezelfde environment kan genereren, zodat je de env folder zelf niet bij je source control hoeft in te checken (deze folder is heel groot is en bevat hele programma's).

The env folder can be generated on basis of a requirements.txt file with below commands. 
```
pip freeze > requirements.txt
```
Records an environment's current package list into requirements. txt
``` 
pip install -r rquirements.txt
```
Installs the specified packages with the specified version. 
Haalt ook requirements op die er niet expliciet instaan, maar dependecies zijn.  

` `  
Per project heb je een requirements file. 
De folder ziet er als volgt uit:
```
v project folder
    > __pycache__
    > .vscode
    > env
      .gitignore
      requirements.txt
      project.py
```
