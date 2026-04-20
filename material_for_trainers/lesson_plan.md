# 🌞 DAY 1 🌞

## 9:00 - Installation check - 20' - CATA 

- 🖥 Early start for people that had trouble with installation
- Tools that should be installed:
    - Python
    - VScode
    - GitHub SSH key

## 9:20 - Land - 10' - CATA 
☕ Coffee/tea 🫖

## 9:30 - Welcome - 5' - CATA 
- ✅ Roll call + 🤝 Code of Conduct
- 🙋 Getting help (🆘 red  ✅ green stickers)

## 9:35 - A short icebreaker - 5' - CATA 
[TODO]

## 9:40 - Introduction - 15' - CATA 
source: [carpentries](https://carpentries-incubator.github.io/python-intermediate-development/instructor/10-section1-intro.html) 

🎦 use [slides](https://tud365.sharepoint.com/:p:/r/sites/ResearchDataServices/Gedeelde%20documenten/Training/Research_Software_Training/lesson_plans/resources/Intermediate%20programming%20with%20Python.pptx?d=w0c2ead6d71874acca3944dcdff26f1f9&csf=1&web=1&e=V2REU6) 

- Course overview
- Tools we will use
- When Jupyter notebooks are the right tool
- When to graduate to a `.py` script
- How this course fits into a researcher's workflow
- Why project structure matters from day one
- Setting the scene


## 9:55 - Fork the repository - 10' - CATA 
source: [carpentries](https://carpentries-incubator.github.io/python-intermediate-development/instructor/11-software-project.html) 

- Go to `README` file in the `edu.nl` link
- Click on `python-intermediate-inflammation` repository link 
- Make sure you are signed into your GitHub account
- Click the `Fork` to create a fork under your GitHub account
- Leave default name `python-intermediate-inflammation`
    - [TODO] only main branches for us!
    - uncheck the `Copy the main branch` only option
- Click on the `<> Code` button
- Click on the `SSH` tab to copy the address of the repository for the next exercise

## 10:05 - 1 💪 Obtain the software project locally - 10' - CATA  
see `exercises.md`

Solution
```bash
cd ~/Desktop/
git clone git@github.com:<YOUR_GITHUB_USERNAME>/python-intermediate-inflammation.git
cd python-intermediate-inflammation/
git remote -v               # if starts with HTTPS, change to SSH
git remote set-url origin git@github.com:<YOUR_GITHUB_USERNAME>/python-intermediate-inflammation.git
git remote -v
```

## 10:15 - Our project structure - 10' - CATA 
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

## 10:25 - 2 💪 Have a peek at the data - 5' - CATA
see `exercises.md`

Solution
```bash
wc -l data/inflammation-01.csv
wc -l data/small-01.csv
```

## 10:30 - Break - 15'

## 10:45 - Virtual Environments For Software Development - 15' - RAUL 
🎦 Introduce Virtual environments with the kitchen analogy using [slides](https://tud365.sharepoint.com/:p:/r/sites/ResearchDataServices/Gedeelde%20documenten/Training/Research_Software_Training/lesson_plans/resources/Intermediate%20programming%20with%20Python.pptx?d=w0c2ead6d71874acca3944dcdff26f1f9&csf=1&web=1&e=V2REU6) 

```bash
cat inflammation/views.py           # numpy and matplotlib external packages
```
- These packages are not installed in the basic version of python. We use a package manager to install `extra` packages.

- If you work on multiple projects, you may need specific versions of each library. For this we use `virtual environments` which create an isolated bubble of the libraries so they don't conflict with each other. A virtual environment is *simply* an isolated directory with their own Python interpreter and packages.

- To clarify the difference between a package and virtual environment manager, continue with the kitchen analogy:
    - **🍳 Package manager:** "What equipment do I need?" (pots, knives, blenders — i.e. numpy, matplotlib)
    - **🏠 Virtual environment:** "Which kitchen am I cooking in?" (so you don't mix up one recipe's equipment with another's)
    - **🥕 Your data:** the ingredients you bring to that kitchen
    - **Your code:** the recipe

- There many options for package managers and venv managers. Some of them (like `conda`) do both roles together. In this course we use `pip` as package manager and `venv` as virtual environment manager

## 11:00 - Creating Virtual Environments Using `venv` - 20' - RAUL 
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

## 11:20 - 💪 Creating Virtual Environments Using `venv` - 10' - RAUL 
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

## 11:30 - Break - 15'

## 11:45 - VSCode orientation - 15' - CATA 
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

## 12:05 - 💪 Requirements file - 10' - CATA 
see `exercises.md`

solution
```bash
python3 -m pip list       # see list of packages
cat requirements.txt      # see contents of requirements.txt -> pytest is missing
python3 -m pip freeze > requirements.txt  # regenerate requirements.txt
python3 -m pip freeze --exclude-editable > requirements.txt # remember the --exclude-editable flag for your current project
```

## 12:15 - Version control using IDE - 10' - CATA 
In the `Version control` view we see a little blue notification. It means `git` has found a change
  - Let's verify that files inside `venv` are not tracked by `git`
  - `venv` automatically adds a `.gitignore` file
  > **explicitly ask** if `venv` is not ignored. If so ask helpers add `venv/*` to `.gitignore`
  - `requirements.txt` is untracked (`U`)
  - click on `+` to stage changes (add new file)
  - Write commit message and click on `Commit`
  - Notice update on log history in the `GRAPH` below
  - Finally, we can `Sync Changes` which means `Pull + Push`.


## 12:20 - Lunch - 60'

## 13:20 - Why testing matters - 10' - RAUL 
source: [carpentries](https://carpentries-incubator.github.io/python-intermediate-development/instructor/21-automatically-testing-software.html#what-is-software-testing)

- 🎦 use [slides](https://tud365.sharepoint.com/:p:/r/sites/ResearchDataServices/Gedeelde%20documenten/Training/Research_Software_Training/lesson_plans/resources/Intermediate%20programming%20with%20Python.pptx?d=w0c2ead6d71874acca3944dcdff26f1f9&csf=1&web=1&e=V2REU6) 
- Why write tests?
  -- Ensure correctness
  -- Catch regressions: new code should not break existing code
- Unit tests, regression tests, integration tests
- How to read a failing test and a stack trace

## 13:30 - Unit testing with Pytest - 20' - RAUL 

```bash
``` 
- Inflammation data analysis
- tests using NumPy testing library
- tests using pytest
- install pytest
- Run tests/test_models.py
- How pytest finds tests: functions named test_

## 13:50 - 💪 Unit testing with Pytest - 15' - RAUL 
- Write Some Unit Tests
- git commit

## 14:05 - Debugging in the IDE - 15' - RAUL 
source: [carpentries](https://carpentries-incubator.github.io/python-intermediate-development/instructor/24-diagnosing-issues-improving-robustness.html#debugging-in-an-ide)

Live demo 
- How to read an error message and a stack trace
- Configure Python tests in VSCode
- Run tests
- Running the debugger
- Breakpoints
- Inspecting variables
- Reading the call stack

## 14:20 - Break - 15'

## 14:35 - Python Coding Style Guide - 15' - CATA 
source: [carpentries](https://carpentries-incubator.github.io/python-intermediate-development/instructor/15-coding-conventions.html)

- 🎦 use [slides](https://tud365.sharepoint.com/:p:/r/sites/ResearchDataServices/Gedeelde%20documenten/Training/Research_Software_Training/lesson_plans/resources/Intermediate%20programming%20with%20Python.pptx?d=w0c2ead6d71874acca3944dcdff26f1f9&csf=1&web=1&e=V2REU6) 
- PEP8
- Quick mention of formatting guidelines and show autopep8 in VScode: (Indentation, Maximum Line Length, Line Break, Blank Lines, Whitespace, String Quotes)
- Spend more time on Naming Conventions: Function, Variable, Class, Module, Package Naming in Python
- Good practices when writting Comments
- Type hints (not in SWC) but super important

## 14:50 - 💪 Python Coding Style Guide - 20' - CATA 
- Set up autopep8 in VSCode
- Improve Code Style of Our Project
- git commit

## 15:10 - Python Coding Style Guide - 5' - CATA 
- 🎦 use [slides](https://tud365.sharepoint.com/:p:/r/sites/ResearchDataServices/Gedeelde%20documenten/Training/Research_Software_Training/lesson_plans/resources/Intermediate%20programming%20with%20Python.pptx?d=w0c2ead6d71874acca3944dcdff26f1f9&csf=1&web=1&e=V2REU6) 
- Documentation Strings aka Docstrings

## 15:15 - Python Coding Style Guide - 20' - CATA 
- Fix the Docstrings
- Add type hints
- git commit

## 15:35 - Break - 15'

## 15:50 - 💪 LAB: Testing the inflammation project - 40' - RAUL 

- Write unit tests for daily_min(), daily_mean(), daily_max()
- Introduce an intentional bug and confirm a test catches it
- Fix the bug and re-run the test suite
- git commit

## 16:30 - Review LAB with the group - 10' - RAUL 
Discussion (face-2-face) 
- Discuss solutions and common issues
- Q&a

## 16:40 - Summarize key points - 10' - RAUL 
- 🎦 use [slides](https://tud365.sharepoint.com/:p:/r/sites/ResearchDataServices/Gedeelde%20documenten/Training/Research_Software_Training/lesson_plans/resources/Intermediate%20programming%20with%20Python.pptx?d=w0c2ead6d71874acca3944dcdff26f1f9&csf=1&web=1&e=V2REU6) 
- Recap: project setup, virtual environments, VSCode, git, testing, debugging, style
- Preview of Day 2: code quality, project structure, refactoring
- Questions


# 🌞 DAY 2 🌞


## 9:10 - Welcome - 5' - RAUL 
- ✅ Roll call + 🤝 Code of Conduct
- 🙋 Getting help (🆘 red  ✅ green stickers)

## 9:15 - Recap from day one - 10' - RAUL 
- 🎦 use [slides](https://tud365.sharepoint.com/:p:/r/sites/ResearchDataServices/Gedeelde%20documenten/Training/Research_Software_Training/lesson_plans/resources/Intermediate%20programming%20with%20Python.pptx?d=w0c2ead6d71874acca3944dcdff26f1f9&csf=1&web=1&e=V2REU6) 
- Key concepts from Day 1: setup, virtual environments, testing, debugging, style
- Questions from participants

## 9:25 - Data validation - 10' - RAUL 
```bash
``` 
- What About Testing for Errors?
- raises()
- Testing for invalid input data -> data validation
- Update requirements file

## 9:35 - 💪 Data validation - 10' - RAUL 
- Write data validation test for daily_mean() and daily_max()
- git commit

## 9:45 - Test parametrization - 10' - RAUL 
```bash
``` 
- Parameterising Our Unit Tests
- Edge cases 

## 9:55 - 💪 Test parametrization - 15' - RAUL 
Write Parameterised Unit Tests (remember to add edge cases)
- git commit

## 10:10 - Break - 15'

## 10:25 - Defensive programming - 10' - CATA 
```bash
``` 
- Check data at input
- Raise errors and fail
- Fix (assumptions) and raise a warning

## 10:35 - 💪 Defensive programming - 15' - CATA 
- Exercise: Add a Precondition to Check the Correct Type and Shape of Data
- git commit

## 10:50 - Abstractions and Decoupling - 15' - CATA 
- 🎦 use [slides](https://tud365.sharepoint.com/:p:/r/sites/ResearchDataServices/Gedeelde%20documenten/Training/Research_Software_Training/lesson_plans/resources/Intermediate%20programming%20with%20Python.pptx?d=w0c2ead6d71874acca3944dcdff26f1f9&csf=1&web=1&e=V2REU6) 
- What is an abstraction?
- Decoupling: why it matters
- Practical tips:
  -- Long chains of ""and"" in a function -> extract an abstraction
  -- Copy/paste code -> extract an abstraction
  
  ## 11:05 - 💪 Abstractions and Decoupling - 15' - CATA 
- Exercise: Decouple Data Loading from Data Analysis
- git commit

## 11:20 - Break - 15'

## 11:35 - Organising code into modules - 15' - RAUL 
- 🎦 use [slides](https://tud365.sharepoint.com/:p:/r/sites/ResearchDataServices/Gedeelde%20documenten/Training/Research_Software_Training/lesson_plans/resources/Intermediate%20programming%20with%20Python.pptx?d=w0c2ead6d71874acca3944dcdff26f1f9&csf=1&web=1&e=V2REU6) 
- What is a Python module?
- When to split Code into separate files
- Imports and namespaces
- Keeping modules focused: one responsibility per module

## 11:50 - 💪 Organising code into modules - 30' - RAUL 
- Split the inflammation project into separate modules (data loading, analysis, reporting)
- Update imports
- git commit

## 12:20 - Open Q&A and catch-up - 10' - RAUL 
- 🎦 use [slides](https://tud365.sharepoint.com/:p:/r/sites/ResearchDataServices/Gedeelde%20documenten/Training/Research_Software_Training/lesson_plans/resources/Intermediate%20programming%20with%20Python.pptx?d=w0c2ead6d71874acca3944dcdff26f1f9&csf=1&web=1&e=V2REU6) 
- Participants Raise questions from the morning
- Catch up on any unfinished exercises

## 12:30 - Lunch - 60'
## 13:30 - Refactoring - 10' - CATA 
- 🎦 use [slides](https://tud365.sharepoint.com/:p:/r/sites/ResearchDataServices/Gedeelde%20documenten/Training/Research_Software_Training/lesson_plans/resources/Intermediate%20programming%20with%20Python.pptx?d=w0c2ead6d71874acca3944dcdff26f1f9&csf=1&web=1&e=V2REU6) 
- Change structure not behaviour.
  -- abstractions, decoupling, renaming, reorganising, reduce duplication DO NOT fix bugs
- Writing Tests Before Refactoring

## 13:40 - 💪 Refactoring - 20' - CATA 
- Write regression tests before refactoring
- git commit

## 14:00 - Separating Pure and Impure Code - 10' - CATA 
- 🎦 use [slides](https://tud365.sharepoint.com/:p:/r/sites/ResearchDataServices/Gedeelde%20documenten/Training/Research_Software_Training/lesson_plans/resources/Intermediate%20programming%20with%20Python.pptx?d=w0c2ead6d71874acca3944dcdff26f1f9&csf=1&web=1&e=V2REU6) 
- Pure functions: same input always gives same output, no side effects
- Benefits of Pure functions for testing and reasoning about code

## 14:10 - 💪 Separating Pure and Impure Code - 20' - CATA 
- Refactor to use a pure function
- git commit

## 14:30 - Break - 15'

## 14:45 - Testing pure functions - 10' - RAUL 
- 🎦 use [slides](https://tud365.sharepoint.com/:p:/r/sites/ResearchDataServices/Gedeelde%20documenten/Training/Research_Software_Training/lesson_plans/resources/Intermediate%20programming%20with%20Python.pptx?d=w0c2ead6d71874acca3944dcdff26f1f9&csf=1&web=1&e=V2REU6) 
- Testing Pure Functions
- Functional programming

## 14:55 - 💪 Testing pure functions - 15' - RAUL 
Testing a Pure Function


## 15:10 - the __main__ function - 10' - RAUL 
```bash
``` 
- __name__  ""__main__"

- Why it matters for scripts vs imports

## 15:20 - Command line arguments - 10' - RAUL 
```bash
``` 
- argparser
- run from terminal
- positional and optional argument order

## 15:30 - 💪 Command line arguments - 15' - RAUL 
- Exercise: Add optional parameter:
  -- a filename for a figure. If paremeter exists, save figure to file insted of plot.show()
- git commit

## 15:45 - Break - 15'

## 16:00 - 💪 LAB refactoring using inflammation_analysis.py - 40' - CATA 
- Add warnings for suspicious data values
- Add optional input parameters
- Ensure all tests still pass
- git commit
- git push

## 16:40 - Summarize key points - 10' - CATA 
- 🎦 use [slides](https://tud365.sharepoint.com/:p:/r/sites/ResearchDataServices/Gedeelde%20documenten/Training/Research_Software_Training/lesson_plans/resources/Intermediate%20programming%20with%20Python.pptx?d=w0c2ead6d71874acca3944dcdff26f1f9&csf=1&web=1&e=V2REU6) 
- Recap: project structure, defensive Programming, Abstractions, refactoring, Pure functions
- Where to go next: packaging, documentation tools (Sphinx), continuous integration
- questions and feedback


## 16:50 - Give feedback about the course - 5'                                        
                                                
# [? for raul] 
- .venv vs venv vs intuitive name
- test windows commands
                                           