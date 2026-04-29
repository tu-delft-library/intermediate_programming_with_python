# 🌞 DAY 1 🌞

##	9:00	-	Installation check	-	20'	-	RAUL

🖥 Tools that should be installed:
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
- Three subfolders:
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
  - install `autodocstring` by Nils Werner
  - search for `GitHub Copilot` -> `Disable (Workspace)`
  - setup format for docstring
    - Open `Code` > `Settings` > `Settings`
    - Search for `docstringFormat`
    - Choose `sphinx-notypes`


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
- first import `daily_max`
- copy/paste `test_daily_mean_integers()` and adapt for `daily_max`


```bash

from inflammation.models import daily_mean, daily_min

def test_daily_max_integers():
    """Test that the min function works for an array of positive intergers.
    """

    test_input = np.array([[1, 2],[3, 4],[5, 6]])
    test_result = np.array([5, 6])
    npt.assert_array_equal(daily_max(test_input), test_result)
```
modify `test_input` and `test_result` to include negative values
```bash

    test_input = np.array([[1, 2, -9], [-3, 4, -2], [-1, 5, -6]])
    test_result = np.array([1, 5, -2])

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

def test_daily_max_string():
    """Test for TypeError when passing strings"""      # Write summary before code clarifies purpose

    with pytest.raises(TypeError):                     # string instead of array give TypeError
        error_expected = daily_max(['hi', 'there'])    # simple input      
```
- Git commit

##	11:55	-	 💪 Data validation	-	10'	-	RAUL
see `exercises.md`


solution:
- copy`test_daily_max_string()`
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
            ([[1, 2], [3, 4], [5, 6]], [5, 6]),
            ([[1, 2, -9], [-3, 4, -2], [-1, 5, -6]], [1, 5, -2]),
        ])

def test_daily_max(test_input, test_result):
    """Test that max function works for an array of positive and negative integers."""
    npt.assert_array_equal(daily_max(test_input), test_result)
```
> **Remember** Git commit!

##	12:30	-	Lunch	-	60'	


##	13:30	-	Python Coding Style Guide	-	15'	-	CATA
source: [carpentries](https://carpentries-incubator.github.io/python-intermediate-development/instructor/15-coding-conventions.html)

🎦 use [slides](https://tud365.sharepoint.com/:p:/r/sites/ResearchDataServices/Gedeelde%20documenten/Training/Research_Software_Training/lesson_plans/resources/Intermediate%20programming%20with%20Python.pptx?d=w0c2ead6d71874acca3944dcdff26f1f9&csf=1&web=1&e=V2REU6) 

Let's get a plugin that helps with PEP8 style
  - Go to the `Extensions` tab 
  - install `flake8` by Microsoft


Note the highlighting of `flake8` in `inflammation-analysis.py`

This is called a `linter` and it verifies code style, not functionality.

##	13:45	-	 💪 Improve Code Style of Our Project -	20'	-	CATA
see `exercises.md`

solution:
There are a few things to fix in `inflammation-analysis.py`:

- Line 24 in `inflammation-analysis.py` is too long. A better style would be to use multiple lines and hanging indent

```bash
# Using hanging indent with the, closing '}' aligned with the start of the multiline contruct
view_data = {
    'average': models.daily_mean(inflammation_data),
    'max': models.daily_max(inflammation_data),
    'min': models.daily_min(inflammation_data)
}
```
this type of indent is commonly used for long function calls, lists, and multiline strings.

- Variable `InFiles` in `inflammation-analysis.py` uses `CapitalisedWords` naming convention which is recommended for class names but not variable names. By convention, variable names should be in lowercase with optional underscores so you should rename the variable `InFiles` to, e.g., `infiles` or `in_files`.

- There are two blank lines starting from line 19 in `inflammation-analysis.py`. Normally, you should not use blank lines in the middle of the code unless you want to separate logical units - in which case only one blank line is used. Note how VSCode is warning us by underlining the whole line below.

- Only one blank line after the end of definition of function `main` and the rest of the code below line 27 in `inflammation-analysis.py` - should be two blank lines (PEP 8 recommends surrounding top-level function (and class) definitions with two blank lines). Note how VSCode is warning us by underlining the whole line below.

> **Remember** Git commit!

##	14:05	-	Python Coding Style Guide	-	10'	-	CATA
source: [carpentries](https://carpentries-incubator.github.io/python-intermediate-development/instructor/15-coding-conventions.html#documentation-strings-aka-docstrings)

[TODO expand]
- Documentation Strings aka Docstrings
- typehints

Let's get ready for the next exercise:
- Open `models.py`
- Option 1:
    - Delete the `docstring` of `daily_mean`
    - Stand below the `def` line and type `"""`
    - A pop up will offer to generate the `docstring`
    - Press `ENTER` and a template of `doscstring` will appear
- Option 2: 
    - Stand where you want the `docstring`
    - Go to `View > Command Palette`, or shortcut `CTRL-SHIFT-P` (Windows, Linux) or `CMD-SHIFT-P` (macOS)
    - Execute `Generate Docstring`

##	14:15	-	💪 Fix the Docstrings	-	15'	-	CATA

see `exercises.md`

solution:
The improved `docstrings` for the above functions would contain explanations for parameters and return values.

```bash
def daily_mean(data):
   """Calculate the daily mean of a 2D inflammation data array for each day.

   :param data: A 2D data array with inflammation data (each row contains measurements for a single patient across all days).
   :return: An array of mean values of measurements for each day.
   """
   return np.mean(data, axis=0)
```
```bash
def daily_max(data):
   """Calculate the daily maximum of a 2D inflammation data array for each day.

   :param data: A 2D data array with inflammation data (each row contains measurements for a single patient across all days).
   :return: An array of max values of measurements for each day.
   """
   return np.max(data, axis=0)
```
```bash
def daily_min(data):
   """Calculate the daily minimum of a 2D inflammation data array for each day.

   :param data: A 2D data array with inflammation data (each row contains measurements for a single patient across all days).
   :return: An array of minimum values of measurements for each day.
   """
   return np.min(data, axis=0)
```
##	14:30	-	Break	-	10'			

##	14:40	-	Debugging in the IDE	-	15'	-	RAUL
source: [carpentries](https://carpentries-incubator.github.io/python-intermediate-development/instructor/24-diagnosing-issues-improving-robustness.html#debugging-in-an-ide)

A bug is an expected behavior of code. Debugging means "finding and fixing a bug". 

The debugger allows us to inspect the internal workings of a programming while it executes. We can see the values of variables.

Let's configure pytest in VSCode. 
- Click on the Testing panel (`flask` view) on the most left column
- Click on `Configure Python Tests` button 
- A pop up should open on the command palette
- Choose `pytest`
- Select folder containing your tests. In our case `tests`
- A list of tests should appear in the testing tab.
    - Note that one test is deliberately broken. So ignore the `pytest discovery error` message
- Run all the tests clicking the ▶️ icon next to the top entry `tests`
- You can run an individual test clicking the ▶️ icon next to the specific test case

Now let's ask the debugger to stop at a specific point. We do this by adding a `breakpoint 🔴` in the **gutter** (the narrow strip just to the left of the line numbers):
- Add a break point in `test_daily_mean` on the line where `npt.assert_array_equal` is executed (e.g. line 19)
- Run `test_daily_mean` with the debugger attached (▶️  + 🐞 )
- VSCode will switch to the **Run and Debug** view and pause execution on the `breakpoint 🔴` line. The editor highlights the current line in yellow.

- Notice on the left view, at the top there is a list of `VARIABLES` and it shows the current values of the variables `test_input` and `test_result`
- Those are the values that are being generated by the `parametrize` function

To continue executing the code we have several options:

| Button | Keyboard | What it does |
|--------|----------|-------------|
| ▶ Continue | `F5` | Resume until the next breakpoint or end |
| ↷ Step Over | `F10` | Run the current line and move to the next |
| ↓ Step Into | `F11` | Step inside the next function to be executed|
| ⏹ Stop | `Shift+F5` | End the debug session |


Let's `Step into` which takes us to the function `daily_mean`:
- Notice the variables still on the upper left column. 
- Use the Debug Console (bottom panel, tab labelled Debug Console) to evaluate expressions interactively:
    - ```np.min(data, axis=0)```
    - ```np.min(data, axis=1)```
    - ```np.max(data, axis=0)```
- Also notice the `CALL STACK`. It shows which function calls the current function.
> **CALL STACK** is how python keeps track of "where did I come from, and where do I go back to?" 

- Call stack is very useful when you need to debug more complex code, one function calls another which calls another
- `Step over` to go back to `test_daily_mean`
- And `Continue` executing and note how the variables change in each iteration


##	14:55	-	 💪 PRACTICAL: Catching bugs	-	45'	-	RAUL
see `PRACTICAL_unit_testing_debugging.md`

##	15:40	-	Review PRACTICAL with the group	-	10'	-	RAUL

- Discuss solutions and common issues
- Q&a

##	15:50	-	Summarize key points	-	10'	-	RAUL
[TODO Vevox]
- Make a poll and use it to recap: virtual environments, testing, debugging, style
- Questions

##	16:15	-	Good bye			

# 🌞 DAY 2 🌞				
##	9:00	-	Coffee, tea	-	10'	-	RAUL

##	9:10	-	Welcome	-	5'	-	RAUL

- ✅ Roll call + 🤝 Code of Conduct
- 🙋 Getting help (🆘 red  ✅ green stickers)

##	9:15	-	Recap from day one	-	10'	-	RAUL	

- Key concepts from Day 1: environments, testing, debugging, style
- Questions from participants

##	9:25	-	 💪 Simulate a contribution from a colleague	-	5'	-	RAUL	
see `exercises.md`

solution:
- Open `exercises.md` 
- Go to `💪 Simulate a contribution from a colleague`
- Follow instructions to generate module with code



##	9:30	-	Abstractions and Decoupling	-	10'	-	RAUL	

source: [carpentries](https://carpentries-incubator.github.io/python-intermediate-development/instructor/33-code-decoupling-abstractions.html#introduction)

- What is decoupling? breaking up the software into smaller components. Two components of code can be considered decoupled if a change in one does not need a change in the other. 
- What is an abstraction? hiding the details of *how* something works leaving us only with *what* it does. 
- Why is it important:
  - easier to read as you only need to understand the details of the (smaller) component you are looking at and not the whole monolithic codebase.
  - easier to test, as one of the components can be replaced by a test or a mock version of it.
  - easier to maintain, as changes can be isolated from other parts of the code.

- When to implement them in practice?
  - when you find your self copy/pasting code, this is a sign that you can turn that bit of code into an abstraction (i.e. an independent function that you can call multiple times)
  - when you name your function and use lots of `and's` (e.g. `load_and_compute_and_save()`) this suggests that you should split this function into smaller functions
  
  
##	9:40	-	 💪 Decouple Data Loading from Data Analysis	-	15'	-	RAUL	

see `exercise.md`


##	9:55	-	Review exercise	-	5'	-	RAUL	

The new function `load_inflammation_data()` that reads all the inflammation data into the format needed for the analysis could look something like: .

```bash
def load_inflammation_data(dir_path):
    data_file_paths = glob.glob(os.path.join(dir_path, 'inflammation*.csv'))
    if len(data_file_paths) == 0:
        raise ValueError(f"No inflammation CSV files found in path {dir_path}")
    data = map(models.load_csv, data_file_paths) # Load inflammation data from each CSV file
    return list(data) # Return the list of 2D NumPy arrays with inflammation data
```
The new function `analyse_data()` could then look like:

```bash
def analyse_data(data_dir):
    data = load_inflammation_data(data_dir)

    means_by_day = map(models.daily_mean, data)
    means_by_day_matrix = np.stack(list(means_by_day))

    daily_standard_deviation = np.std(means_by_day_matrix, axis=0)

    graph_data = {
        'standard deviation by day': daily_standard_deviation,
    }
    views.visualize(graph_data)
```
The code is now easier to follow since we do not need to understand the data loading part to understand the statistical analysis part, and vice versa. In most cases, functions work best when they are short!

##	10:00	-	Encapsulations and Classes	-	15'	-	RAUL	
source: [carpentries](https://carpentries-incubator.github.io/python-intermediate-development/instructor/33-code-decoupling-abstractions.html#encapsulation-classes
)

However, even with the change done in the previous exercise the data loading is still coupled with the data analysis to a large extent. For example, if we have to support loading data from different sources (e.g. `JSON` files or an `SQL` database), we would have to pass some kind of a flag into `analyse_data()` indicating the type of data we want to read from. Instead, we would like to decouple the consideration of data source from the `analyse_data()` function entirely. 

One way we can do this is by using *encapsulation*. Encapsulation can be used to group together data with methods that manipulate that data. 

For example a `class` is a very common encapsulation.

> **Analogy** A `class` is a cookie cutter template, and `instances` as the cookies themselves. That is, one class can have many instances.

*Note* adapting Circle example to Patient to be relevant for this course. 

Open the module `models`

```bash
class Circle:           # how to declare a class 
  pass                 # notice the name convention > CapitalisedWords
```

You can construct an instance of a class elsewhere in the code by doing the following:
```bash
my_circle = Circle()    # instance of `Circle` is assigned the variable `my_circle`
```
When you construct a class, the class’ *constructor* method is called.
- `__init__` is special name of the constructor
- `self` access current instance of object being created

```bash
class Circle:
  def __init__(self, radius):   # indentation marks code encapsulated in the class
    self.radius = radius        # assign input parameter to current instance     

my_circle = Circle(10)          # no indentation 
```
Class can have other methods (aka functions) defined on them. 
```bash
import math

class Circle:
  ...
  def get_area(self):         # self paramenter is required
    return math.pi * self.radius * self.radius
...
print(my_circle.get_area())
```

On the last line of the code above, the instance of the class, `my_circle`, will be automatically passed as the first parameter (`self`) when calling the `get_area()` method. The `get_area()` method can then access the variable `radius` encapsulated within the object, which is otherwise invisible to the world outside of the object. The method `get_area()` itself can also be accessed via the object/instance only.

Let's run this in VSCode using VSCode (upper right corner `\>` icon). We see the output in the integrated terminal. 
```bash
/.../venv/bin/python /.../inflammation/sandbox.py   # the call 
314.1592653589793                                   # the print out of circle area
```
You can also run it directly in the terminal typing
```bash
python inflammation/sandbox.py 
```

> **Key concept** Encapsulation provides information hiding. Abstraction provides implementation hiding.


##	10:15	-	 💪 Use Classes to Abstract out Data Loading	-	15'	-	RAUL

see `exercise.md`

##	10:30	-	Review exercise	-	5'	-	RAUL	

in `compute_data.py`

```bash
class CSVDataSource:
    """
    Loads all the inflammation CSV files within a specified directory.
    """
    def __init__(self, dir_path):
        self.dir_path = dir_path

    def load_inflammation_data(self):
        data_file_paths = glob.glob(os.path.join(self.dir_path, 'inflammation*.csv'))
        if len(data_file_paths) == 0:
            raise ValueError(f"No inflammation CSV files found in path {self.dir_path}")
        data = map(models.load_csv, data_file_paths)
        return list(data)
```

in `inflammation-analysis.py`
```bash
data_source = CSVDataSource(os.path.dirname(infiles[0]))
analyse_data(data_source)
```

The `analyse_data()` function is modified to receive any data source object (that implements the `load_inflammation_data()` method) as a parameter.

```bash
def analyse_data(data_source):
    data = data_source.load_inflammation_data()
    ...
```
We have now fully decoupled the reading of the data from the statistical analysis and the analysis is not fixed to reading from a directory of CSV files. Indeed, we can pass various data sources to this function now, as long as they implement the `load_inflammation_data()` method.

While the overall behaviour of the code and its results are unchanged, the way we invoke data analysis has changed.

##	10:35	-	Break	-	15'

##	10:50	-	💪  Add an Additional DataSource	-	20'	-	CATA	

see `exercise.md`


##	11:10	-	Review exercise	-	5'	-	CATA	

The class that reads inflammation data from JSON files could look something like:
```bash
class JSONDataSource:
  """
  Loads patient data with inflammation values from JSON files within a specified folder.
  """
  def __init__(self, dir_path):
    self.dir_path = dir_path

  def load_inflammation_data(self):
    data_file_paths = glob.glob(os.path.join(self.dir_path, 'inflammation*.json'))
    if len(data_file_paths) == 0:
      raise ValueError(f"No inflammation JSON files found in path {self.dir_path}")
    data = map(models.load_json, data_file_paths)
    return list(data)
```
in `inflammation-analysis.py`
```bash
_, extension = os.path.splitext(infiles[0])
if extension == '.json':
  data_source = JSONDataSource(os.path.dirname(infiles[0]))
elif extension == '.csv':
  data_source = CSVDataSource(os.path.dirname(infiles[0]))
else:
  raise ValueError(f'Unsupported data file format: {extension}')
analyse_data(data_source)
```

##	11:15	-	Refactoring	-	10'	-	CATA	
source: [carpentries](https://carpentries-incubator.github.io/python-intermediate-development/instructor/34-code-refactoring.html#introduction)

Code *refactoring* is the process of improving the design of an existing code: **change structure not behaviour**.

We have already been refactoring: adding abstractions, decoupling, renaming, reorganising, reducing duplication

When you refactor, contain the urge to fix bugs. You need to be able to run the code an generate the SAME output so that you can verify nothing is broken. If you find a bug: you make a note for future you!

Before we refactor, we should have tests that can verify the code behaviour as it is now. A common strategy is `test at a higher level`, with coarser accuracy. These type of tests are called *regression tests*

We'll write this test in the next exercise. This is the plan: we will modify the function to return the data instead of visualising it because graphs are harder to test automatically (i.e. they need to be viewed and inspected manually in order to determine their correctness). Next, we will make the assert statements verify what the current outcome is, rather than check whether that is correct or not.


##	11:25	-	💪 Write Regression Tests	-	20'	-	CATA	

see `exercise.md`

##	11:45	-	Review exercise	-	5'	-	CATA	

One approach we can take is to:

- comment out the visualise method on `analyse_data()` (this will cause our test to hang waiting for the result data)
- return the data (instead of plotting it on a graph), so we can write assert statements on the data
- see what the calculated result value is, and assert that it is the same as the expected value
Putting this together, our test may look like:

```bash
import numpy.testing as npt
from inflammation.compute_data import analyse_data

def test_analyse_data():
    path = os.path.join( os.getcwd(), "../data")
    data_source = CSVDataSource(path)
    result = analyse_data(data_source)
    expected_output = [0.,0.22510286,0.18157299,0.1264423,0.9495481,0.27118211,
                       0.25104719,0.22330897,0.89680503,0.21573875,1.24235548,0.63042094,
                       1.57511696,2.18850242,0.3729574,0.69395538,2.52365162,0.3179312,
                       1.22850657,1.63149639,2.45861227,1.55556052,2.8214853,0.92117578,
                       0.76176979,2.18346188,0.55368435,1.78441632,0.26549221,1.43938417,
                       0.78959769,0.64913879,1.16078544,0.42417995,0.36019114,0.80801707,
                       0.50323031,0.47574665,0.45197398,0.22070227]
    npt.assert_array_almost_equal(result, expected_output)
```

Note that while the above test will detect if we accidentally break the analysis code and change the output of the analysis, it is still not a complete test for the following reasons:

It is not obvious why the `expected_output` is correct
It does not test edge cases
If the data files in the directory change - the test will fail
We would need to add additional tests to check the above.

##	11:50	-	Break	-	15'

##	12:05 -	Separating Pure and Impure Code	-	10'	-	RAUL	
source: [carpentries](https://carpentries-incubator.github.io/python-intermediate-development/instructor/34-code-refactoring.html#separating-pure-and-impure-code)

Next step is to separate out as much of code as possible into *pure functions*.

The output of a pure function does not depend on any information which is not present in the input (such as global variables). The same input always gives same output, no side effects.

Pure functions are easier to:
- understand because they eliminate side effects.
- reuse as the caller only needs to understand what parameters to provide
- test 

##	12:15	-	💪 Separating Pure and Impure Code	-	20'	-	RAUL	

see `exercise.md`

##	12:35	-	Review exercise	-	5'	-	RAUL	

The analysis code will be refactored into a separate function that may look something like:

```bash
def compute_standard_deviation_by_day(data):
    means_by_day = map(models.daily_mean, data)
    means_by_day_matrix = np.stack(list(means_by_day))

    daily_standard_deviation = np.std(means_by_day_matrix, axis=0)
    return daily_standard_deviation
```
The `analyse_data()` function now calls the `compute_standard_deviation_by_day()` function, while keeping all the logic for reading the data, processing it and showing it in a graph:

```bash
def analyse_data(data_dir):
    """Calculates the standard deviation by day between datasets.
    Gets all the inflammation data from CSV files within a directory, works out the mean
    inflammation value for each day across all datasets, then visualises the
    standard deviation of these means on a graph."""
    data_file_paths = glob.glob(os.path.join(data_dir, 'inflammation*.csv'))
    if len(data_file_paths) == 0:
        raise ValueError(f"No inflammation csv's found in path {data_dir}")
    data = map(models.load_csv, data_file_paths)
    daily_standard_deviation = compute_standard_deviation_by_day(data)

    graph_data = {
        'standard deviation by day': daily_standard_deviation,
    }
    # views.visualize(graph_data)
    return daily_standard_deviation
```
Make sure to re-run the regression test to check this refactoring has not changed the output of `analyse_data()`.

##	12:40	-	Lunch	-	60'

##	13:40	-	The __main__ function and command line arguments	-	15'	-	CATA	

You will have noticed already that structure of the `inflammation-analysis.py `file follows this pattern:
```bash
# import modules

def main(args):
    # perform some actions

if __name__ == "__main__":
    # perform some actions before main()
    main(args)
```

In this pattern the actions performed by the script are contained within the main function (which does not need to be called main, but using this convention helps others in understanding your code). The main function is then called within the if statement __name__ == "__main__", after some other actions have been performed (usually the parsing of command-line arguments, which will be explained below). __name__ is a special dunder variable which is set, along with a number of other special dunder variables, by the python interpreter before the execution of any code in the source file. What value is given by the interpreter to __name__ is determined by the manner in which it is loaded. 

If we run the source file directly using the Python interpreter, e.g.:

```bash
$ python3 inflammation-analysis.py
```
then the interpreter will assign the hard-coded string "__main__" to the __name__ variable:

```bash
__name__ = "__main__"
...
# rest of your code
```
However, if your source file is imported by another Python script, e.g:

```bash
import inflammation-analysis
```
then the interpreter will assign the name "inflammation-analysis" from the import statement to the __name__ variable:

```bash
__name__ = "inflammation-analysis"
...
# rest of your code
```
Because of this behaviour of the interpreter, we can put any code that should only be executed when running the script directly within the if __name__ == "__main__": structure, allowing the rest of the code within the script to be safely imported by another script if we so wish.

While it may not seem very useful to have your controller script importable by another script, there are a number of situations in which you would want to do this:

for testing of your code, you can have your testing framework import the main script, and run special test functions which then call the main function directly;
where you want to not only be able to run your script from the command-line, but also provide a programmer-friendly application programming interface (API) for advanced users.

[TODO expand]
- argparse basics
- Positional and optional arguments
- Run from terminal

##	13:55	-	 💪 Add optional input parameter	-	15'	-	CATA	

see `exercise.md`

##	14:10	-	Review exercise	-	5'	-	CATA	

[TODO]

##	14:15	-	Organising code into modules	-	10'	-	CATA	

- What is a Python module?
- When to split Code into separate files
- Imports and namespaces
- Keeping modules focused: one responsibility per module

##	14:25	-	Refactoring: Organising code into modules	-	15'	-	CATA	

see `exercise.md`
[TODO] rethink this part. Code is already modular. Do we just rename the modules? or is there more separation we can do?
[OPTIONAL] fix test_patient test. Requires adding a class inside `models.py` called `Patient` initilaized with a `name`. 
- Add other properties (weight, height, age)
- Add a method to compute BMI 
- To convert weight between kg and pounds
- To convert hegiht between mts and feet

##	14:40	-	Review exercise	-	5'	-	CATA	

Review exercise

##	14:45	-	Break	-	15'	


##	15:00	-	💪 PRACTICAL -	40'	-	RAUL	

options: defensive programming or more refactoring
- Add preconditions to check correct type and shape of input data
- Raise errors and fail early
- Add warnings for suspicious data values
- Confirm tests still pass

##	15:40	-	Final  review	-	10'	-	RAUL	

- Discuss solutions and common issues
- Q&a

##	15:50	-	Summarize key points	-	10'	-	RAUL	

- Recap: Project structure, abstractions, modules, Pure functions, refactoring
- Where to go next: packaging, documentation Tools (Sphinx), continuous integration
- questions and feedback

##	15:00	-	Give feedback about the course	-	5'	-	RAUL	

##	16:05 -	Good bye

# [? for raul]
- test windows commands -> will do on his computer
- add to email -> this course is not for you if you use pytorch

