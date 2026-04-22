# 🌞 DAY 1 🌞

##	9:00	-	Installation check	-	20'	-	RAUL

- 🖥 Tools that should be installed:
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

🎦 Use [slides](https://tud365.sharepoint.com/:p:/r/sites/ResearchDataServices/Gedeelde%20documenten/Training/Research_Software_Training/lesson_plans/resources/Intermediate%20programming%20with%20Python.pptx?d=w0c2ead6d71874acca3944dcdff26f1f9&csf=1&web=1&e=V2REU6) 

- Why write tests?
  - Ensure correctness: expected errors in our code
  - Testing both valid and invalid input (data validation)
  - Catch regressions: new code should not break existing code
- The three main types of automated: 
  - unit tests: small and specific functionality (e.g. a function returns an expected output)
  - integration tests: higher level. Test interaction between functions
  - regression tests: whole pipelines. Ensures output has not changed. Normally requires input data.


##	11:15	-	Unit testing with Pytest	-	10'	-	CATA
source: [carpentries](https://carpentries-incubator.github.io/python-intermediate-development/instructor/21-automatically-testing-software.html#using-a-testing-framework)

Let's open the file `inflammation/models.py`. Note function `daily_mean()` which calculates mean *vertically* across the data.

Open `tests/test_models.py`. In here we have two `test cases`. Each test has an input, an execution and an expected output. A test case is simple and easy to understand. 

The name of the test is important:
- `test_` -> starts with `test_` so that `pytest` can find it
- `daily_mean` -> the funciton its testing
- `_zeros`-> specific input

Let's run the tests using `pytest`

```bash
pytest tests/test_models.py  # run one file (same as python3 -m pytest tests/test_models.py)
```

Now, what happens when the test we wrote is not working properly? Let's deliberately break this test
```bash
def test_daily_mean_integers():
    """Test that mean function works for an array of positive integers."""

    test_input = np.array([[1, 2],
                           [3, 4],
                           [5, 6]])
    # delete test_result
    npt.assert_array_equal(daily_mean(test_input), test_result)         # test_result is not defined
```
When we run the tests again, we see an error trace
```bash
__________________________ test_daily_mean_integers_broken ________________________

    def test_daily_mean_integers():
        """Test that mean function works for an array of positive integers."""
    
        test_input = np.array([[1, 2],
                               [3, 4],
                               [5, 6]])
>       npt.assert_array_equal(daily_mean(test_input), test_result)
                                                       ^^^^^^^^^^^
E       NameError: name 'test_result' is not defined

tests/test_models.py:29: NameError
```
Read it top to bottom:

- `test_daily_mean_integers_broken` — tells you which test failed
- `> arrow` — marks the exact line that failed
- `E lines` — the error message, what `pytest` expected vs. got
- `File path + line number — tests/test_models.py:39` — where to go fix it

Fix the test before continuing:

```bash
test_result = np.array([3, 4])
```
> **OPTIONAL** Use git to reverse broken test (using discard changes)

##	11:25	-	💪 Unit testing with Pytest	-	10'	-	CATA
see `exercises.md`


solution:
- first import `daily_min`
- copy/paste `test_daily_mean_integers()` and adapt for `daily_min`


```bash

from inflammation.models import daily_mean, daily_min

def test_daily_min_integers():
    """Test that the min function works for an array of positive intergers.
    """

    test_input = np.array([[1, 2],
                           [3, 4],
                           [5, 6]])
    test_result = np.array([1, 2])
    npt.assert_array_equal(daily_min(test_input), test_result)
```
modify `test_input` and `test_result` to include negative values
```bash

    test_input = np.array([[ 4, -2, 5],
                           [ 1, -6, 2],
                           [-4, -1, 9]])
    test_result = np.array([-4, -6, 2])

```
> **Remember** Git commit!

##	11:35	-	Break	-	10'


##	11:45	-	Data validation	-	10'	-	RAUL
source: [carpentries](https://carpentries-incubator.github.io/python-intermediate-development/instructor/21-automatically-testing-software.html#what-about-testing-for-errors)

Testing the behaviour of inputs, both valid and invalid, is a really good idea and is known as *data validation*.

Python allows to test for invalid data and `raise` an error. 
The most common errors we can use for data validation:

- `TypeError` — wrong type of input (e.g. passing `None` or a string instead of an array)
- `IndexError` — accessing an index that doesn't exist (e.g. requesting `axis 0` on an empty array)
- `ValueError` — right type, wrong value or shape (e.g. an array with incompatible dimensions) 

```bash
import pytest                                          # import `pytest` to use `raises()` 

def test_daily_min_string():
    """Test for TypeError when passing strings"""      # Write summary before code clarifies purpose

    with pytest.raises(TypeError):                     # string instead of array give TypeError
        error_expected = daily_min(['hi', 'there'])    # simple input      
```
- Git commit

##	11:55	-	 💪 Data validation	-	10'	-	RAUL
see `exercises.md`


solution:
- copy`test_daily_min_string()`
- Paste it right below other  `daily_mean` tests (so that tests are clustered by function)
- adapt to use `daily_mean`
```bash
def test_daily_mean_string():
    """Test that the mean function fails for an array of strings
    """
    with pytest.raises(TypeError):
        error_expected = daily_mean(['hi','there'])
```

> **Remember** Git commit!

##	12:05	-	Test parametrization	-	10'	-	RAUL
source: [carpentries](https://carpentries-incubator.github.io/python-intermediate-development/instructor/22-scaling-up-unit-testing.html#parameterising-our-unit-tests)

Let's take a look at our tests so far. They seem to have a lot of the same lines of code. The main difference is the input/result arrays.

We can make this code more efficient by *parametrizing* the tests with multiple test inputs. We add a *python decorator* right above the test function we want to use.
```bash
@pytest.mark.parametrize(                       # python decorator
        "test_input, test_result",                # name of arguments
        [
            ([ [0, 0], [0, 0], [0, 0] ], [0, 0]),   # values of arguments
            ([ [1, 2], [3, 4], [5, 6] ], [3, 4]),
        ])

def test_daily_mean(test_input, test_result):
    """Test that mean function works for an array of zeros and positive integers."""
    npt.assert_array_equal(daily_mean(test_input), test_result)
```
After this is working we can use this test to investigate *edge cases*: unexpected circumstances or extreme input values. 

For example:

- All zeros — handles zero matrix correctly
- Single row — return that row unchanged
```bash
@pytest.mark.parametrize(
        "test_input, test_result",
        [
            ([ [0, 0], [0, 0], [0, 0] ], [0, 0]),
            ([ [1, 2], [3, 4], [5, 6] ], [3, 4]),
            (np.zeros((3, 5)), np.zeros(5)),
            ([[1, 2, 3]], [1, 2, 3]),
        ])
```

Once the tests are passing, we can **commit!**


##	12:15	-	 💪 Test parametrization	-	15'	-	RAUL

see `exercises.md`

solution:
```bash
@pytest.mark.parametrize(
        "test_input, test_result",
        [
            ([ [0, 0, 0], [0, 0, 0], [0, 0, 0] ], [0, 0, 0]),
            ([ [1, 2, -1],[3, -2, 4],[5, -9, 6]], [1,-9,-1]),
        ])

def test_daily_min(test_input, test_result):
    """Test that min function works for an array of positive and negative integers."""
    npt.assert_array_equal(daily_min(test_input), test_result)
```
> **Remember** Git commit!

##	12:30	-	Lunch	-	60'	


##	13:30	-	Python Coding Style Guide	-	15'	-	CATA
source: [carpentries](https://carpentries-incubator.github.io/python-intermediate-development/instructor/15-coding-conventions.html)

🎦 use [slides](https://tud365.sharepoint.com/:p:/r/sites/ResearchDataServices/Gedeelde%20documenten/Training/Research_Software_Training/lesson_plans/resources/Intermediate%20programming%20with%20Python.pptx?d=w0c2ead6d71874acca3944dcdff26f1f9&csf=1&web=1&e=V2REU6) 

- [TODO expand] 
  - PEP8
  - Formatting guidelines and show autopep8 in VScode: (Indentation, Maximum Line Length, Line Break, Blank Lines, Whitespace, String Quotes)
  - Naming Conventions: Function, Variable, Class, Module, Package Naming in Python
  - Good practices when writting Comments
  - Set up autopep8 in VSCode

##	13:45	-	 💪 Python Coding Style Guide	-	20'	-	CATA
see `exercises.md`

solution:
There are a few things to fix in `inflammation-analysis.py`:

- Line 30 in `inflammation-analysis.py` is too long. A better style would be to use multiple lines and hanging indent

```bash
# Using hanging indent with the, closing '}' aligned with the start of the multiline contruct
view_data = {
    'average': models.daily_mean(inflammation_data),
    'max': models.daily_max(inflammation_data),
    'min': models.daily_min(inflammation_data)
}
```
- Variable `InFiles` in `inflammation-analysis.py` uses `CapitalisedWords` naming convention which is recommended for class names but not variable names. By convention, variable names should be in lowercase with optional underscores so you should rename the variable `InFiles` to, e.g., `infiles` or `in_files`.

- There are two blank lines starting from line 19 in `inflammation-analysis.py`. Normally, you should not use blank lines in the middle of the code unless you want to separate logical units - in which case only one blank line is used. Note how VSCode is warning us by underlining the whole line below.

- Only one blank line after the end of definition of function main and the rest of the code below line 27 in inflammation-analysis.py - should be two blank lines (PEP 8 recommends surrounding top-level function (and class) definitions with two blank lines). Note how VSCode is warning us by underlining the whole line below.

> **Remember** Git commit!

##	14:05	-	Python Coding Style Guide	-	5'	-	CATA
source: [carpentries](https://carpentries-incubator.github.io/python-intermediate-development/instructor/15-coding-conventions.html#documentation-strings-aka-docstrings)

[TODO expand]
- Documentation Strings aka Docstrings
- typehints

##	14:10	-	💪 Fix the Docstrings	-	20'	-	CATA

see `exercises.md`

solution:
The improved docstrings for the above functions would contain explanations for parameters and return values.

```bash
def daily_mean(data):
   """Calculate the daily mean of a 2D inflammation data array for each day.

   :param data: A 2D data array with inflammation data (each row contains measurements for a single patient across all days).
   :returns: An array of mean values of measurements for each day.
   """
   return np.mean(data, axis=0)
```
```bash
def daily_max(data):
   """Calculate the daily maximum of a 2D inflammation data array for each day.

   :param data: A 2D data array with inflammation data (each row contains measurements for a single patient across all days).
   :returns: An array of max values of measurements for each day.
   """
   return np.max(data, axis=0)
```
```bash
def daily_min(data):
   """Calculate the daily minimum of a 2D inflammation data array for each day.

   :param data: A 2D data array with inflammation data (each row contains measurements for a single patient across all days).
   :returns: An array of minimum values of measurements for each day.
   """
   return np.min(data, axis=0)
```
##	14:30	-	Break	-	10'			

##	14:40	-	Debugging in the IDE	-	15'	-	RAUL
source: [carpentries](https://carpentries-incubator.github.io/python-intermediate-development/instructor/24-diagnosing-issues-improving-robustness.html#debugging-in-an-ide)

[TODO expandß]
- Configure Python tests in VSCode
- Run tests
- Running the debugger
- Breakpoints
- Inspecting variables
- Reading the call stack

##	14:55	-	 💪 PRACTICAL: Testing the inflammation project	-	45'	-	RAUL
[TODO]
Consolidation PRACTICAL:
- Introduce an intentional bug and confirm a test catches it
- Use the debugger to locate the bug
- Fix the bug and re-run the test suite
- git commit

##	15:40	-	Review PRACTICAL with the group	-	10'	-	RAUL

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
source: [carpentries](https://carpentries-incubator.github.io/python-intermediate-development/instructor/33-code-decoupling-abstractions.html#introduction)

[TODO expand]
- What is an abstraction?
- Decoupling: why it matters
- Practical tips:
  -- Long chains of and in a function -> extract an abstraction
  -- Copy/paste code -> extract an abstraction

##	9:45	-	 💪 Decouple Data Loading from Data Analysis	-	15'	-	CATA

- Exercise: Decouple Data Loading from Data Analysis
- git commit

##	10:00	-	Encapsulations and Classes	-	15'	-	CATA
source: [carpentries](https://carpentries-incubator.github.io/python-intermediate-development/instructor/33-code-decoupling-abstractions.html#encapsulation-classes
)

[TODO expand]
- Classes
- naming -> CapitalisedWords
- instances 
- __init__
- self

##	10:15	-	 💪 Use Classes to Abstract out Data Loading	-	15'	-	CATA

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
- we are not using branches because we didn't want to exclude people that have not done the interim git course
  - this affects for lessons in episode 3 (DAY 2). carpentries requires to switch branch. So that a [compute_data.py](https://github.com/carpentries-incubator/python-intermediate-inflammation/blob/full-data-analysis/inflammation/compute_data.py) is added. How can we deal with this? I don't want work with branches in this course. 
  - we can also make a modified version of the materials that includes this file. Have participants fork that one instead.

- Other topics to add:
 - Configuration management - YAML configs
 - Logging vs. printing (logging module)
 - Data handling > can we do it without introducing pandas?