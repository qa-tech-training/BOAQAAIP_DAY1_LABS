### Lab PY00 - Setting up the Development Environment

#### Objective
The goal of this lab is to set up a Python development environment under WSL, accessible via VSCode

#### Outcomes
By the end of this lab, you will have:
* Configured VSCode with extensions
* Connected VSCode to a WSL environment
* Installed the necessary Python tooling within a WSL environment

#### High-Level Steps
* Configuring VSCode
* Connecting to WSL
* Installing Python toolchain

#### Detailed Steps
##### Configuring VSCode
1. Open VSCode from the Windows Search bar. VSCode is a popular, flexible multi-language IDE which provides an extensive ecosystem of _extensions_ for enabling wider integrations
2. Open the extensions tab from the VSCode side menu  
![VSCode extensions icon](image.png)
3. Search for and install the 'WSL' extension. Wait for it to install, then close and relaunch VSCode to ensure the extension is activated

##### Connecting to WSL
4. In the bottom left of the VSCode window, click the blue 'open a remote window' icon, and from the menu that appears choose 'connect to WSL'. If prompted, select the remote platform as 'Linux'. A new VSCode window will load, connected to your WSL environment on the classroom machine
5. In this new window, click the extensions icon in the side menu again. This time, search for 'Python' - install the Python extension from Microsoft. Make sure when installing to select 'install in remote: WSL'.
6. Once the extension is installed, click the file explorer icon in the menu, and click 'open folder'. Open your home directory (/home/qa). VSCode will reload the window and open your home directory in the file explorer. 

##### Installing Python Tooling
7. Open a new integrated terminal in VSCode, by pressing ctrl+'. This will open a terminal with the working directory set to whatever your current open directory in VSCode is - in this case /home/qa, which will probably be represented in your prompt by a '~' (tilde)
8. This WSL environment is running Ubuntu, a common distribution of Linux. Python should already be installed. Verify this by executing the following command in your terminal:
```bash
python3 --version
```
9. We will also need two additional pieces of Python tooling installed (for now, there will be more to come later). Specifically, we will be using the Python package manager, pip, and the (v)irtual (env)ironment - or _venv_ module. Install these (note: the _sudo_ password in these environments is simply qa):
```bash
sudo apt-get update
sudo apt-get install python3-pip python3-venv
```
10. Verify the installation:
```bash
pip3 --version
python3 -m venv --help
```

##### Cloning This Repository
This repo contains some starter files and a directory structure which corresponds to the instructions. Clone it into your environment like so:
```bash
git clone https://github.com/qa-tech-training/BOAQAAIP_DAY1_LABS.git Labs1
```
### Lab PY01 - Python Introduction

#### Objective
Create and run your first Python script

#### Outcomes
By the end of this lab, you will have:
* Written a python script which is capable of basic i/o and string manipulation
* Executed said script

#### High-Level Steps
- Setup a new project workspace with virtual environment
- Create and run a new Python script

#### Detailed Steps
##### Set up a python project
1. In the integrated terminal in VSCode, change directory into the PY01 directory: 
```shell
cd ~/Labs1/PY01 
```
2. Expand this directory in the VSCode file explorer. The directory contains a single, empty file, main.py. 
3. We could start working on this file straight away, however it is good to embed best practices from the beginning, so we will first create a _virtual environment_:
```shell
python3 -m venv venv 
source venv/bin/activate 
```
Observe that your prompt now begins (venv), indicating that the virtual environment is active. The purpose of the virtual environment is to allow us to install any dependencies with a limited scope - unlike tools like, say, NPM, pip implicitly installs packages globally unless a venv is used. This would be problematic if we required different versions of the same package across different projects.
4. Now we need a python script. Using the file explorer, open the main.py file, and enter the following contents:  
```python
#! /home/qa/Labs/PY01/venv/bin/python3
if __name__ == '__main__':
    print("Hello World!")
```
##### Basic Script - Breakdown
Broadly speaking, this script has three components: 
```python     
#! /home/qa/Labs/PY01/venv/bin/python3 
```
is what is known as a 'shabang'. This is used to allow the script to be invoked as if it were any other executable file on linux systems - the shabang is ignored on Windows, and by the python interpreter itself. In this instance, we have used the venv instance of python3, to ensure that any active venv configuration is respected. 
```python
if __name__ == '__main__': 
```
As with the use of venv, this is overkill in the context of this exercise. It is a matter of best practice, however, for reasons that will reveal themselves in a later module, so we will include it here. 
```python
print("Hello World!") 
```
is the actual logic of the script - for now, a simple print statement. We will flesh this out momentarily.

5. Run the script, to verify that all is working as intended: 
```shell
./main.py 
```
You should see "Hello World!" printed in the terminal  
NOTE: Thanks to the Python extension, VSCode will detect that the file is a python file and allow you to run the script via the IDE, using the 'play button' icon. For this exercise this will not affect the outcome, but in later labs it may cause problems as it will not autodetect the venv. Invoking the script via the shell will always work.

##### Work with some fundamental data types
6. We will now make our script do something slightly more complex, in order to demonstrate some ways of interacting with common data types. Modify the body of your main.py script to do the following: 
- asks a user to input a first name and a last name, and saves these to two string variables 
- prints the provided name out, in the format LASTNAME, Firstname 
- prints whether the last name ends with '-son' 
- sums the total number of vowels in both names and prints the result 
- prints the square of the difference between the lengths of the first and last name
```python
#! venv/bin/python3

if __name__ == '__main__':
    first_name = input("Enter a first name: ")
    last_name = input("Enter a last name: ")
    print(f"{last_name.upper()}, {first_name.capitalize()}")
    print(last_name.lower().endswith('son'))
    full_name = first_name.lower() + " " + last_name.lower()
    total_vowels = full_name.count('a') + full_name.count('e') + full_name.count('i') + full_name.count('o') + full_name.count('u')
    print(total_vowels)
    print((len(first_name) - len(last_name)) ** 2)
``` 
```shell
./main.py
```

##### Using CLI Help
7. In reality, when implementing scripts, it is unlikely that someone has given you a document like this with all the code snippets you need. As such, you will need to be able to solve problems and investigate the usage of functions and modules for yourself.
8. In the integrated terminal, run `python3 -i` to start an interactive Python interpreter session.
9. Start by reviewing the documentation for the `print` function we used in the script above:
```python
help(print)
```
10. Next, review the documentation for one of the string methods used above:
```python
help(str.lower)
```
11. Note that instead of a single function, you can also review the documentation of an entire class or module - e.g.
```python
help(str)
```
12. End the interactive session using the exit function:
```python
exit()
```

#### Optional Stretch Tasks
- modify the script to take the firstname and lastname as a single string, of the format "firstname,lastname", and extract the two variables from this string (hint: see the help for the string.split method)

### Lab PY02 - Flow Control and Error Handling

#### Objective
Create a simple terminal-based number game in python, with error handling for common issues

#### Outcomes
By the end of this lab, you will have:
* Implemented flow control, iteration and error handling
* Experienced iterative development

#### High-Level Steps
- Setup a new project with venv
- Script core game logic
- Implement error handling on user input
- Implement replays

#### Detailed Steps
##### Setup the Project
1. Ensure you have an active VSCode session connected to WSL - refer back to Lab PY00 if you do not, and cannot remember the steps

2. From the integrated terminal within VSCode, change into the PY02 directory: 
```shell     
cd ~/Labs1/PY02
```
and expand this directory in the VSCode file explorer.
3. Setup a new virtual environment for the project:
```shell     
python3 -m venv venv     
source venv/bin/activate
```
Open the main.py file, which contains a basic placeholder script: 
```python
#! venv/bin/python3
if __name__ == '__main__':
    print("TODO: Implement me")
```

##### Implement a simple game loop
4. For this exercise, we will aim to build a simple guessing game using the control flow constructs we have examined. The game logic itself is reasonably simple: 
- a random number is chosen 
- the player inputs integers (positive or negative) which will be summed together. The aim is to have this sum hit the target number
- The player has 10 inputs to hit the target 
Start by implementing this fundamental game loop:
```python
#! venv/bin/python3
from random import randint

if __name__ == '__main__':
    total = 0
    target = randint(0, 100) # a random number is chosen
    print("Objective: enter up to 10 numbers which sum to a randomly chosen target value")
    for move in range(0, 10): # 10 moves to hit the target
        uinp = input("Enter a number: ") # user enters a number
        inp = int(uinp)
        total += inp # inputs are summed together
        if total == target: # the aim is to hit the target
            print(f"Target hit! Moves taken: {move + 1}")
            break # stop the game if the user has 'won'
        print(f"Current total: {total} is {'greater' if total > target else 'lower'} than target")
```
5. Run the script, and play the game
```bash
./main.py
# play the game by entering inputs
``` 
does it behave as expected?

##### Implement error handling
6. This simple game loop is a good starting point, but has possible failure points. Consider for example the cast to an integer - if the provided input cannot be parsed as an integer a ValueError exception will be thrown - this could crash the program. 
7. Add some error handling for this case using try ... except ...:
```python
#! venv/bin/python3
from random import randint

if __name__ == '__main__':
    total = 0
    target = randint(0, 100) 
    print("Objective: enter up to 10 numbers which sum to a randomly chosen target value")
    for move in range(0, 10): 
        uinp = input("Enter a number: ") 
        try: # add this line
            inp = int(uinp) # indent this and next line
            total += inp 
        except ValueError: # add this and next two lines
            print(f"{uinp} is not a valid integer")
            continue
        if total == target: 
            print(f"Target hit! Moves taken: {move + 1}")
            break 
        print(f"Current total: {total} is {'greater' if total > target else 'lower'} than target")
```
8. Run the script again, and try entering non-numeric inputs - they should be gracefully handled without crashing the program

##### Implement the ability to play multiple rounds
Currently, we can play one round of this game a time before the script exits - we will now implement the ability to continue playing. 
9. Wrap the existing game logic in an infinite while loop, and give the player an choice to continue playing at the end of each iteration:
```python
#! venv/bin/python3
from random import randint

if __name__ == '__main__':
    while True: # <- add this line, indent following lines
        total = 0
        target = randint(0, 100) 
        print("Objective: enter up to 10 numbers which sum to a randomly chosen target value")
        for move in range(0, 10): 
            uinp = input("Enter a number: ") 
            try:
                inp = int(uinp) 
                total += inp 
            except ValueError: 
                print(f"{uinp} is not a valid integer")
                continue
            if total == target: 
                print(f"Target hit! Moves taken: {move + 1}")
                break 
            print(f"Current total: {total} is {'greater' if total > target else 'lower'} than target")
        if input("Play again(y/n): ").lower() == "n": # add this line
            break # and this one
```

### Lab PY03 - Functions and Modules

#### Objective
Implement a vat calculator function, which can be imported into and used by another script

#### Outcomes
By the end of this lab, you will have:
* Created a python function
* Used type-hinting and docstrings to document said function
* Imported said function into another script

#### High-Level Steps
- Setup a new workspace + venv
- Create a new Python file which defines a vat calculator function
- Document the function
- Make the function part of a module and import it into another script

#### Detailed Steps
##### Set up a python project
1. From the integrated terminal, change into the PY03 directory and set up a project venv:
```shell
    cd ~/Labs1/PY03
    python3 -m venv venv
    source venv/bin/activate
``` 
2. Expand this new directory in the VSCode file explorer. Using the file explorer, open the calculator.py file.

##### Create a function
3. Add the following contents to calculator.py:
```python
#! venv/bin/python3

def calculate_total(subtotal, rate=20):
    vat = (rate / 100) * subtotal
    return subtotal + vat

if __name__ == '__main__':
    print(calculate_total(100, 20))
```
4. Execute the script:
```shell
./calculator.py
```
The output should, of course, be 120.0.

##### Document your function
5. Now we will edit our function to be properly documented. Make the following changes to the calculator.py file:
```python
#! venv/bin/python3

def calculate_total(subtotal: float, rate: float = 20) -> float: # add the type hints on this line, and the following multi-line string
    '''
    calcluate total amount, given subtotal and vat rate
    vat rate should be provided as a percentage
    >>> calculate_total(100, 20)
    120.0
    >>> calculate_total(10, 50)
    15.0
    '''
    vat = (rate / 100) * subtotal
    return subtotal + vat

if __name__ == '__main__':
    print(calculate_total(100, 20))
```
6. Run the script again - the behaviour should not have changed, the information we have added is purely documentational.
7. Review the documentation we added in VSCode - hover over the name of the calculate_total function. Notice that VSCode has automatically detected the documentation we added.

##### Creating a custom module
8. We will now make our new functions available as part of a module. Create a new directory called vat_calculator:
```shell
mkdir vat_calculator
```
9. Move the calculator.py file into this directory, and also create an \_\_init\_\_.py file:
```shell
mv calculator.py vat_calculator
touch vat_calculator/__init__.py
```
10. Create a new file in the current directory, main.py, and add the following contents:
```python
#! venv/bin/python3
from vat_calculator.calculator import calculate_total

if __name__ == '__main__':
    print(calculate_total(100, 20))
```
11. Make this script executable and run it:
```shell
chmod +x main.py
./main.py
```

#### Optional Stretch Tasks
- Add a docstring to the \_\_init\_\_.py file explaining the purpose of the module, and use the pydoc utility to generate html-formatted documentation for your module

### Lab PY04 - Working With Files

#### Objective
Create a script which uses all the concepts introduced so far to perform validations against a particular type of configuration file

#### Outcomes
By the end of this lab, you will have:
* Implemented file I/O in a python script
* Used libraries to parse common data formats
* Implemented real-world useful automation via python

#### High-Level Steps
- Setup project workspace and venv
- Parse a YAML file into a python object
- Use flow control concepts to implement validation for the YAML file
- Write validation results as a JSON file

#### Detailed Steps
##### Set up the python project
1. From the integrated terminal within VSCode, switch into the PY04 directory and set up the project venv: 
```shell     
cd ~/Labs1/PY04
python3 -m venv venv
source venv/bin/activate
```
2. Open this directory in the VSCode file explorer and open the main.py file:
```python 
#! venv/bin/python3
if __name__ == '__main__':
    print("TODO: Implement me")
```

##### Install PyYAML and parse a yaml file
For this exercise, we will build a script to validate Kubernetes Pod manifests against certain criteria, and save the validation results as a JSON file. To do this, we will need to be able to parse YAML files.
1. begin by installing the PyYAML library:
```shell
venv/bin/python3 -m pip install pyyaml
```
2. in main.py, implement the ability to parse a YAML file:
```python
#! venv/bin/python3
import yaml

if __name__ == '__main__':
    with open('pod.yaml', 'r') as podfile:
        pod = yaml.safe_load(podfile)
        print(pod)
```
3. Save the file. Run your python script:
```shell
./main.py
```
Observe the output. Note the structure of the dictionary object into which the YAML has been parsed.

##### Implement validation logic
4. In main.py, define a function which validates the following criteria: 
- the image used by each container begins with a given prefix 
- any container ports lie within a given range 
and, in your if name main block, call this function with the dictionary object as an argument:
```python
#! venv/bin/python3
import yaml

def validate_pod(pod, prefix, port_range):
    valid = True
    for container in pod["spec"]["containers"]: # could be more than one container per pod
        if not container.get("image", "").startswith(prefix): # validate that the image has a given registry prefix
            valid = False
        for port in container.get("ports", dict()): # could be multiple, or no, ports defined
            if not (port_range[0] <= port.get("containerPort", 0) <= port_range[1]): # validate that the containerPort is within the defined range
                valid = False
    return valid

if __name__ == '__main__':
    with open('pod.yaml', 'r') as podfile:
        pod = yaml.safe_load(podfile)
        validation = validate_pod(pod, prefix="my-registry.example.io/", port_range=(5000, 10000)) # call the function
        print(validation)
```
5. Run your script again. The validation should come back as False.

##### Generate JSON results file
6. We are now able to validate the criteria we wish to check against, but currently we have no way of knowing which criteria are and are not passing the checks. We will now have our script produce a results file, in JSON format, detailing which validations have passed and failed. 
7. Edit your main.py file again:
```python
#! venv/bin/python3
import yaml
import json # add this import

def validate_pod(pod, prefix, port_range):
    valid = True
    results = {"validation results": []} # create dict to hold results
    with open("status_report.json", 'w') as outfile: # open json file
        for container in pod["spec"]["containers"]:
            if not container.get("image", "").startswith(prefix):
                valid = False
                results['validation results'].append({f'{container.get("name")} image validation': {'status': 'failed', 'reason': f'image {container.get("image")} not from expected registry: {prefix}'}}) # add the validation result and reason to outputs
            else:
                results['validation results'].append({f'{container.get("name")} image validation': {'status': 'passed', 'reason': f'image {container.get("image")} from expected registry: {prefix}'}})
            for port in container.get("ports", dict()):
                if not (port_range[0] <= port.get("containerPort", 0) <= port_range[1]):
                    valid = False
                    results['validation results'].append({f'{container.get("name")} port validation': {'status': 'failed', 'reason': f'port {port.get("containerPort")} not in range {port_range[0]} - {port_range[1]}'}})
                else:
                    results['validation results'].append({f'{container.get("name")} port validation': {'status': 'passed', 'reason': f'port {port.get("containerPort")} in required range {port_range[0]} - {port_range[1]}'}})
        json.dump(results, outfile, indent=2) # write out the results
        return valid
```
8. We should also update the if name main block so that the output directs the user as to where to find detailed results:
```python
if __name__ == '__main__':
    with open('pod.yaml', 'r') as podfile:
        pod = yaml.safe_load(podfile)
        validation = validate_pod(pod, prefix="my-registry.example.io/", port_range=(5000, 10000))
        if validation:
            print("all validations passed")
        else:
            print("validation failed - see status_report.json for details")
```

#### Optional Stretch Tasks
- Amend the script to read the filename, registry prefix and port range from the command-line, instead of hard-coding (hint: the built-in _argparse_ library may be useful)
- Amend the script to also populate a global CSV file with the path to each validated file, its' validation status and number of failed validations (hint: support for reading/writing CSV files is available via the built-in _csv_ library)
- Amend the script to check whether the input yaml file defines a Pod or a Deployment - if it's a deployment, modify the logic of the script to pass just the pod part of the spec to validate_pod