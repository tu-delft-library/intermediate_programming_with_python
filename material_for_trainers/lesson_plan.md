# 🌞 DAY 1 🌞

## 9:00 - Installation check - 20' - CATA 

- 🖥 Early start for people that had trouble with installation
- Tools that should be installed:
    - Python
    - VScode
    - GitHub SSH key
    - `carpentries` environment [todo]

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


## 10:00 - Fork the repository - 10' - CATA 
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

## 10:10 - 1 💪 Obtain the software project locally - 10' - CATA  
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

## 10:20 - Our project structure - 10' - CATA 
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

## 10:30 - 2 💪 Have a peek at the data - 5' - CATA
see `exercises.md`

Solution
```bash
wc -l data/inflammation-01.csv
wc -l data/small-01.csv
```

## 10:35 - Break - 15'

## 10:50 - Virtual Environments For Software Development - 15' - RAUL 
🎦 Introduce Virtual environments with the kitchen analogy using [slides](https://tud365.sharepoint.com/:p:/r/sites/ResearchDataServices/Gedeelde%20documenten/Training/Research_Software_Training/lesson_plans/resources/Intermediate%20programming%20with%20Python.pptx?d=w0c2ead6d71874acca3944dcdff26f1f9&csf=1&web=1&e=V2REU6) 

```bash
cat inflammation/views.py           # numpy and matplotlib external packages
```
- These packages are not installed in the basic version of python. We use a package manager to install `extra` packages.

- If you work on multiple projects, you may need specific versions of each library. For this we use `virtual environments` which create an isolated bubble of the libraries so they don't conflict with each other. A virtual environment is *simply* an isolated directory with their own Python interpreter and packages.

- To clarify the difference between a package and virtual environment manager, continue with the kitchen analogy:
    - **🍳 Package manager:** "What equipment do I need?" (pots, knives, blenders — i.e. numpy, matplotlib)
    - **🏠 Virtual environment:** "Which kitchen am I cooking in?" (so you don't mix up one chef's equipment with another's)
    - **🥕 Your data:** the actual ingredients you bring to that kitchen


- In this course we use `pip` as package manager and `venv` as virtual environment manager

## 11:05 - Creating Virtual Environments Using `venv` - 20' - RAUL 
**CATA continue here**
```bash
``` 
    - Creating a Virtual environment with `venv`
    - Naming conventions
    - Activate and check: which python3
    - Deactivate
    - Installing external packages with pip
    - Installing our local Project as a package
    - Exporting environment: pip freeze > requirements.txt
    - Running Python scripts from the command line

## 11:25 - 💪 Creating Virtual Environments Using `venv` - 15' - RAUL 
- Create a new environment
- Activate it
- Install a package
- Export requirements.txt
- Deactivate and go back to system Python

## 11:40 - Break - 15'

## 11:55 - VSCode orientation - 15' - CATA 
Live demo 
- What is an IDE and Why use one
- Opening the Project in VSCode
- Adding the Python interpreter
- Syntax highlighting, code completion, code search
- Running code from the IDE
- We will use VSCode throughout the rest of the Course

## 12:10 - 💪 Requirements file - 10' - CATA 
Exercise: Update requirements file after adding a new dependency
## 12:20 - Version control with VSCode - 10' - CATA 
```bash
``` 
"[Use VSCode for all git operations from here]
- Checking in changes to our project
- Adding venv to .gitignore
- Update and commit requirements.txt
- Sync with remote: pull and push

## 12:30 - Lunch - 60'
## 13:30 - Why testing matters - 10' - RAUL 
- 🎦 use [slides](https://tud365.sharepoint.com/:p:/r/sites/ResearchDataServices/Gedeelde%20documenten/Training/Research_Software_Training/lesson_plans/resources/Intermediate%20programming%20with%20Python.pptx?d=w0c2ead6d71874acca3944dcdff26f1f9&csf=1&web=1&e=V2REU6) 
- Why write tests?
  -- Ensure correctness
  -- Catch regressions: new code should not break existing code
- Unit tests, regression tests, integration tests
- How to read a failing test and a stack trace

## 13:40 - Unit testing with Pytest - 20' - RAUL 
```bash
``` 
- Inflammation data analysis
- tests using NumPy testing library
- tests using pytest
- install pytest
- Run tests/test_models.py
- How pytest finds tests: functions named test_

## 14:00 - 💪 Unit testing with Pytest - 15' - RAUL 
- Write Some Unit Tests
- git commit

## 14:15 - Debugging in the IDE - 15' - RAUL 
Live demo 
- How to read an error message and a stack trace
- Configure Python tests in VSCode
- Run tests
- Running the debugger
- Breakpoints
- Inspecting variables
- Reading the call stack

## 14:30 - Break - 15'

## 14:45 - Python Coding Style Guide - 15' - CATA 
- 🎦 use [slides](https://tud365.sharepoint.com/:p:/r/sites/ResearchDataServices/Gedeelde%20documenten/Training/Research_Software_Training/lesson_plans/resources/Intermediate%20programming%20with%20Python.pptx?d=w0c2ead6d71874acca3944dcdff26f1f9&csf=1&web=1&e=V2REU6) 
- PEP8
- Quick mention of formatting guidelines and show autopep8 in VScode: (Indentation, Maximum Line Length, Line Break, Blank Lines, Whitespace, String Quotes)
- Spend more time on Naming Conventions: Function, Variable, Class, Module, Package Naming in Python
- Good practices when writting Comments
- Type hints (not in SWC) but super important

## 15:00 - 💪 Python Coding Style Guide - 20' - CATA 
- Set up autopep8 in VSCode
- Improve Code Style of Our Project
- git commit

## 15:20 - Python Coding Style Guide - 5' - CATA 
- 🎦 use [slides](https://tud365.sharepoint.com/:p:/r/sites/ResearchDataServices/Gedeelde%20documenten/Training/Research_Software_Training/lesson_plans/resources/Intermediate%20programming%20with%20Python.pptx?d=w0c2ead6d71874acca3944dcdff26f1f9&csf=1&web=1&e=V2REU6) 
- Documentation Strings aka Docstrings
## 15:25 - Python Coding Style Guide - 20' - CATA 
- Fix the Docstrings
- Add type hints
- git commit

## 15:45 - Break - 15'

## 16:00 - 💪 LAB: Testing the inflammation project - 40' - RAUL 

- Write unit tests for daily_min(), daily_mean(), daily_max()
- Introduce an intentional bug and confirm a test catches it
- Fix the bug and re-run the test suite
- git commit

## 16:40 - Review LAB with the group - 10' - RAUL 
Discussion (face-2-face) 
- Discuss solutions and common issues
- Q&a

## 16:50 - Summarize key points - 10' - RAUL 
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
                                                
                                            