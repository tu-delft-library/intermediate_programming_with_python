# 🌞 DAY 1 🌞

##	9:00	-	Installation check	-	20'	-	RAUL

- 🖥 Early start for people that had trouble with installation
- Tools that should be installed:
    - Python
    - VScode
    - GitHub SSH key
    - Forked and cloned `python-intermediate-inflammation`
    
##	9:20	-	Welcome	-	5'	-	RAUL

- ✅ Roll call + 🤝 Code of Conduct
- 🙋 Getting help (🆘 red  ✅ green stickers)

##	9:25	-	A short icebreaker	-	5'	-	RAUL
[TODO]

##	9:30	-	Introduction	-	15'	-	RAUL
source: [carpentries](https://carpentries-incubator.github.io/python-intermediate-development/instructor/10-section1-intro.html) 

🎦 use [slides](https://tud365.sharepoint.com/:p:/r/sites/ResearchDataServices/Gedeelde%20documenten/Training/Research_Software_Training/lesson_plans/resources/Intermediate%20programming%20with%20Python.pptx?d=w0c2ead6d71874acca3944dcdff26f1f9&csf=1&web=1&e=V2REU6) 

- Course overview
- Tools we will use
- When Jupyter notebooks are the right tool
- When to graduate to a `.py` script
- How this course fits into a researcher's workflow
- Why project structure matters from day one
- Setting the scene

##	9:45	-	Our project structure	-	5'	-	RAUL
- Root folder:
    - `inflammation-analysis.py` main entry point
    - `README`
    - `LICENSE`
    - `pyproject.toml` configuration file for dependencies (more on this later)
- Three subfolder:
    - data -> csv files
    - tests -> a few tests (more on this later)
    - inflammation -> has more `.py` scripts
```bash
ls -lF      # detailed list of the contents
ls tests
ls inflammation/
```

##	9:50	-	2 💪 Have a peek at the data - 5' - RAUL
see `exercises.md`

Solution
```bash
wc -l data/inflammation-01.csv
wc -l data/small-01.csv
```

##	9:55	-	Virtual Environments For Software Development	-	10'	-	RAUL
🎦 Introduce Virtual environments with the kitchen analogy using [slides](https://tud365.sharepoint.com/:p:/r/sites/ResearchDataServices/Gedeelde%20documenten/Training/Research_Software_Training/lesson_plans/resources/Intermediate%20programming%20with%20Python.pptx?d=w0c2ead6d71874acca3944dcdff26f1f9&csf=1&web=1&e=V2REU6) 

```bash
cat inflammation/views.py           # numpy and matplotlib external packages
```
- These packages are not installed in the basic version of python. We use a package manager to install `extra` packages.

- If you work on multiple projects, you may need specific versions of each library. For this we use `virtual environments` which create an isolated bubble of the libraries so they don't conflict with each other. A virtual environment is *simply* an isolated directory with their own Python interpreter and packages.

- To clarify the difference between a package and virtual environment manager, continue with the kitchen analogy:
    - **🍳 Package manager:** What equipment do I need? (pots, knives, blenders — i.e. numpy, matplotlib)
    - **🏠 Virtual environment:** Which kitchen am I cooking in? (so you don't mix up one recipe's equipment with another's)
    - **🥕 Your data:** the ingredients you bring to that kitchen
    - **Your code:** the recipe

- There many options for package managers and venv managers. Some of them (like `conda`) do both roles together. In this course we use `pip` as package manager and `venv` as virtual environment manager

##	10:05	-	Creating Virtual Environments Using `venv`	-	15'	-	RAUL

source: [carpentries](https://carpentries-incubator.github.io/python-intermediate-development/instructor/12-virtual-environments.html)

How to create virtual environments
```bash
pwd                         # ensure standing in python-intermediate-inflammation/
python3 -m venv venv       # path convention venv (hidden)
ls -l                       # new  folder venv
ls -l venv                  # bin for python interpreter (Scripts for Windows)
ls venv/lib/pythonX.X/site-packages/    # independent python packages
ls venv\Lib\site-packages               # on windows
source venv/bin/activate                # activate (enter kitchen) 
                                        # note name (venv) before $
source venv/Scripts/activate            # for windows
which python3                           # notice full path inside venv
deactivate                              # exit environment (exit kitchen)
```
How to install packages
```bash
source venv/bin/activate                # reactivate to continue working
python3 -m pip install numpy            # pip install
python3 -m pip install matplotlib
python3 -m pip install numpy>=1.2       # set minimum version of package 
python3 -m pip show numpy               # display info of package
python3 -m pip list                     # list all packages in venv
python3 -m pip uninstall matplotlib     # just demo. Answer n
```
How to install our local project as a package. Allows to call the Python code we are writing from another location.
```bash
python3 -m pip install --editable .     # --editable change dynamically as we develop, . current dir
python3 -m pip install --upgrade pip    # if above fails -> update pip          
python3 -m pip list                     # note our package on the list
```
How to replicate an environment. For your colleagues (and future self) to be able to reproduce your environment with all its dependencies. 
```bash
python3 -m pip freeze --exclude-editable > requirements.txt    # produce list of packages, exclude our package, save into a txt file
cat requirements.txt                # see the list of packages with versions!
```
The `requirements.txt` file can be committed to version control and used later to recreate the environment like this:
```bash
python3 -m pip install -r requirements.txt --editable .
```
How to run Python scripts from the command line
```bash
pwd                         # ensure standing in python-intermediate-inflammation/ 
python3 inflammation-analysis.py    # use python3 to run script in current directory  
python3 inflammation-analysis.py data/inflammation-01.csv # add input file
```

##	10:20	-	💪 Creating Virtual Environments Using `venv` - 10' - RAUL 
see `exercises.md`

solution
```bash
cd ~/Desktop/
mkdir sandbox
cd sandbox/
python3 -m venv venv
ls -al
source venv/bin/activate
python3 -m pip install numpy
python3 -m pip install requests
python3 -m pip list
python3 -m pip freeze > requirements.txt
cat requirements.txt 
deactivate
cd ~/Desktop/python-intermediate-inflammation/
```

##	10:30	-	Break	-	10'	

##	10:40	-	VSCode orientation	-	15'	-	CATA
source: [carpentries](https://carpentries-incubator.github.io/python-intermediate-development/instructor/13-ides.html)

#### Starting with a software project
An Integrated Development Environments (IDEs) is an application with tools to help software development. We will use VS Code.
- Open folder -> navigate to `~/Desktop/python-intermediate-inflammation/`
- Icons on the left bar access key views of VS Code.  
- In the `Explorer` tab we see the project structure (browse through the folders).
  
#### Installing extensions
VSCode is light and general. Which means you need to install what you need.
  - Go to the `Extensions` tab 
  - install `Python` by Microsoft
  - install `autoDocstring` by Nils Werner
  - search for `GitHub Copilot` -> `Disable (Workspace)`

#### Adding a Python interpreter
First we need to tell the IDE which version of Python we want to use. We do that in the terminal
- Open `Terminal` > `New Terminal` and type `source /venv/bin/activate`
- Next step should not be necessary, but it's good to check
  - Go to `View > Command Palette`, or shortcut `CTRL-SHIFT-P` (Windows, Linux) or `CMD-SHIFT-P` (macOS)
  - Search for `Python: Select Interpreter` 
  - Select the one in `Workspace` -> the `venv` we set up earlier today
- Open `inflammation-analysis.py` in code editor
  - We can check that the *correct* python interpreter is installed in the lower right corner. 
- Notice the Python syntax highlighting: comments, special language words (`import`, `from`, `if`), modules, function names, strings.

#### Adding external dependency
To add an external dependency (an extra package) we use the integrated terminal
- Go back to the terminal
- Type `python3 -m pip install pytest`

##	10:55	-	💪 Requirements file - 5' - CATA 
see `exercises.md`

solution
```bash
python3 -m pip list       # see list of packages
cat requirements.txt      # see contents of requirements.txt -> pytest is missing
python3 -m pip freeze > requirements.txt  # regenerate requirements.txt
python3 -m pip freeze --exclude-editable > requirements.txt # remember the --exclude-editable flag for your current project
```

##	11:00	-	Version control using IDE - 10' - CATA 
source: [carpentries](https://carpentries-incubator.github.io/python-intermediate-development/instructor/13-ides.html#version-control)

In the `Version control` view we see a little blue notification. It means `git` has found a change
  - Let's verify that files inside `venv` are not tracked by `git`
  - `venv` automatically adds a `.gitignore` file
  > **explicitly ask** if `venv` is not ignored. If so ask helpers add `venv/*` to `.gitignore`
  - `requirements.txt` is untracked (`U`)
  - click on `+` to stage changes (add new file)
  - Write commit message and click on `Commit`
  - Notice update on log history in the `GRAPH` below
  - Finally, we can `Sync Changes` which means `Pull + Push`.
  
  ##	11:10	-	Why testing matters	-	5'	-	CATA

source: [carpentries](https://carpentries-incubator.github.io/python-intermediate-development/instructor/21-automatically-testing-software.html#what-is-software-testing)

[slides](https://tud365.sharepoint.com/:p:/r/sites/ResearchDataServices/Gedeelde%20documenten/Training/Research_Software_Training/lesson_plans/resources/Intermediate%20programming%20with%20Python.pptx?d=w0c2ead6d71874acca3944dcdff26f1f9&csf=1&web=1&e=V2REU6) 

- Why write tests?
  - Ensure correctness: expected errors in our code
  - Testing both valid and invalid input (data validation)
  - Catch regressions: new code should not break existing code
- The three main types of automated: 
  - unit tests: small and specific functionality (e.g. a function returns an expected output)
  - integration tests: higher level. Test interaction between functions
  - regression tests: whole pipelines. Ensures output has not changed. Normally requires input data.


##	11:15	-	Unit testing with Pytest	-	10'	-	CATA

Let's open the file `inflammation/models.py`. Note function `daily_mean()` which calculates mean *vertically* across the data.

We can test this function as shown in `tests/test_models.py`. In here we have two `test cases`. Each test has an input, an execution and an expected output. A test case is simple and easy to understand. 

The name of the test is important
`test_` -> starts with `test_` so that `pytest` can find it
`daily_mean` -> the funciton its testing
`_zeros`-> specific input

Let's run the tests using `pytest`

```bash
pytest tests/test_models.py  # run one file (same as python3 -m pytest tests/test_models.py)
```


##	11:25	-	💪 Unit testing with Pytest	-	10'	-	CATA
see `exercises.md`

> **While participants work** unplug screen and copy paste these functions in your local copy of `tests/test_models.py`

solution:
```bash

from inflammation.models import daily_max, daily_mean, daily_min
def test_daily_max():
    """Test that max function works for an array of positive integers."""

    test_input = np.array([[4, 2, 5],
                           [1, 6, 2],
                           [4, 1, 9]])
    test_result = np.array([4, 6, 9])

    npt.assert_array_equal(daily_max(test_input), test_result)


def test_daily_min():
    """Test that min function works for an array of positive and negative integers."""

    test_input = np.array([[ 4, -2, 5],
                           [ 1, -6, 2],
                           [-4, -1, 9]])
    test_result = np.array([-4, -6, 2])

    npt.assert_array_equal(daily_min(test_input), test_result)
```

##	11:35	-	Break	-	10'


##	11:45	-	Data validation	-	10'	-	RAUL

- What About Testing for Errors?
- raises()
- Testing for invalid input data -> data validation
- Update requirements file

##	11:55	-	 💪 Data validation	-	10'	-	RAUL

Exercise: 
- Write data validation test for daily_mean() and daily_max()
- git commit

##	12:05	-	Test parametrization	-	10'	-	RAUL

- Parameterising Our Unit Tests
- Edge cases 

##	12:15	-	 💪 Test parametrization	-	15'	-	RAUL

Exercise: Write Parameterised Unit Tests (remember to add edge cases)
- git commit


##	12:30	-	Lunch	-	60'	


##	13:30	-	Python Coding Style Guide	-	15'	-	CATA
source: [carpentries](https://carpentries-incubator.github.io/python-intermediate-development/instructor/15-coding-conventions.html)

- 🎦 use [slides](https://tud365.sharepoint.com/:p:/r/sites/ResearchDataServices/Gedeelde%20documenten/Training/Research_Software_Training/lesson_plans/resources/Intermediate%20programming%20with%20Python.pptx?d=w0c2ead6d71874acca3944dcdff26f1f9&csf=1&web=1&e=V2REU6) 
- PEP8
- Formatting guidelines and show autopep8 in VScode: (Indentation, Maximum Line Length, Line Break, Blank Lines, Whitespace, String Quotes)
- Naming Conventions: Function, Variable, Class, Module, Package Naming in Python
- Good practices when writting Comments

##	13:45	-	 💪 Python Coding Style Guide	-	20'	-	CATA

Exercise: 
- Set up autopep8 in VSCode
- Improve Code Style of Our Project
- git commit

##	14:05	-	Python Coding Style Guide	-	5'	-	CATA

- Documentation Strings aka Docstrings

##	14:10	-	💪 Python Coding Style Guide	-	20'	-	CATA

Exercise: 
- Fix the Docstrings
- Add type hints
- git commit

##	14:30	-	Break	-	10'			

##	14:40	-	Debugging in the IDE	-	15'	-	RAUL
source: [carpentries](https://carpentries-incubator.github.io/python-intermediate-development/instructor/24-diagnosing-issues-improving-robustness.html#debugging-in-an-ide)

- How to read an error message and a stack trace
- Configure Python tests in VSCode
- Run tests
- Running the debugger
- Breakpoints
- Inspecting variables
- Reading the call stack

##	14:55	-	 💪 LAB: Testing the inflammation project	-	45'	-	RAUL

Consolidation lab:
- Introduce an intentional bug and confirm a test catches it
- Use the debugger to locate the bug
- Fix the bug and re-run the test suite
- git commit

##	15:40	-	Review LAB with the group	-	10'	-	RAUL

- Discuss solutions and common issues
- Q&a

##	15:50	-	Summarize key points	-	10'	-	RAUL

- Recap: virtual environments, testing, debugging, style
- Preview of Day 2:  abstractions, refactoring
- Questions

##	16:00	-	Good bye			

# 🌞 DAY 2 🌞				

##	9:15	-	Coffee, tea	-	5'	-	CATA

##	9:20	-	Welcome	-	5'	-	CATA

- ✅ Roll call + 🤝 Code of Conduct
- 🙋 Getting help (🆘 red  ✅ green stickers)

##	9:25	-	Recap from day one	-	5'	-	CATA

- Key concepts from Day 1: environments, testing, debugging, style
- Questions from participants

##	9:30	-	Abstractions and Decoupling	-	15'	-	CATA

- What is an abstraction?
- Decoupling: why it matters
- Practical tips:
  -- Long chains of and in a function -> extract an abstraction
  -- Copy/paste code -> extract an abstraction

##	9:45	-	 💪 Abstractions and Decoupling	-	15'	-	CATA

- Exercise: Decouple Data Loading from Data Analysis
- git commit

##	10:00	-	Encapsulations and Classes	-	15'	-	CATA

- Classes
- naming -> CapitalisedWords
- instances 
- __init__
- self

##	10:15	-	 💪 Encapsulations and Classes	-	15'	-	CATA

- Exercise: Use Classes to Abstract out Data Loading
- git commit

##	10:30	-	Break	-	15'

##	10:45	-	Organising code into modules	-	10'	-	RAUL

- What is a Python module?
- When to split Code into separate files
- Imports and namespaces
- Keeping modules focused: one responsibility per module

##	10:55	-	 💪 Organising code into modules	-	20'	-	RAUL

Exercise:
- Split the inflammation project into separate modules (data loading, analysis, reporting)
- Update imports
- git commit

##	11:15	-	Separating Pure and Impure Code	-	15'	-	RAUL

- Pure functions: same input always gives same output, no side effects
- Benefits of Pure functions for testing and reasoning about code

##	11:30	-	 💪 Separating Pure and Impure Code	-	15'	-	RAUL

Exercise: Refactor to use a pure function
- git commit

##	11:45	-	Break	-	15'	

##	12:00	-	The __main__ function and command line arguments	-	15'	-	CATA

- __name__  __main__
- argparse basics
- Positional and optional arguments
- Run from terminal

##	12:15	-	 💪 The __main__ function and command line arguments	-	15'	-	CATA

- Exercise: Add optional parameter:
  -- a filename for a figure. If paremeter exists, save figure to file insted of plot.show()
- git commit

##	12:30	-	Lunch	-	60'

##	13:30	-	Refactoring	-	10'	-	RAUL

- Change structure not behaviour.
  -- abstractions, decoupling, renaming, reorganising, reduce duplication DO NOT fix bugs
- Writing Tests Before Refactoring

##	13:40	-	 💪 Refactoring exercise	-	20'	-	RAUL

Exercise: Write regression tests before refactoring
- git commit

##	14:00	-	Review	-	5'	-	RAUL

- Instructor-led debrief

##	14:05	-	 💪 Refactoring exercise	-	20'	-	RAUL

Exercise: Rename variables and functions for clarity
- Identify misleading or ambiguous names
- Rename throughout and confirm tests still pass
- git commit

##	14:25	-	Review	-	5'	-	RAUL

- Instructor-led debrief
- common Naming patterns in research code

##	14:30	-	Break	-	15'		

##	14:45	-	 💪 Refactoring exercise	-	25'	-	CATA

Exercise: Find repeated logic in the codebase
- Extract shared logic into reusable functions
- Confirm tests still pass
- git commit

##	15:10	-	Review	-	5'	-	CATA

- Instructor-led debrief

##	15:15	-	 💪 Refactoring exercise	-	25'	-	CATA

Exercise: Revisit the module structure from the morning
- Move functions to the most logical module
- Ensure imports are clean and consistent
- Confirm tests still pass
- git commit

##	15:40	-	Review	-	5'	-	CATA

- Instructor-led debrief

##	15:45	-	Break	-	15'

##	16:00	-	 💪 Optional stretch: defensive programming	-	25'	-	RAUL

Optional exercise (for participants who have finished):
- Add preconditions to check correct type and shape of input data
- Raise errors and fail early
- Add warnings for suspicious data values
- Confirm tests still pass
- git commit
- git push

##	16:25	-	Final  review	-	10'	-	RAUL

- Discuss the refactoring across all  exercises
- What changed? What stayed the same?
- common pitfalls

##	16:35	-	Summarize key points	-	10'	-	RAUL

- Recap: Project structure, abstractions, modules, Pure functions, refactoring
- Where to go next: packaging, documentation Tools (Sphinx), continuous integration
- questions and feedback

##	16:45	-	Give feedback about the course	-	5'	-	RAUL		

##	16:50 - Good bye 
                                        
# [? for raul] 
- .venv vs venv vs intuitive name
- test windows commands

# pending topics

- How to read a failing test and a stack trace