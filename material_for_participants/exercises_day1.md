# Exercises

## 1 💪 Have a peek at the data

Using the integrated `Terminal` in VS code, answer these two questions:

- How many rows does `inflammation-01.csv` have?
- How many rows does `small-01.csv` have?

<details>
<summary>🔍 Click here for hints! </summary>history

- Use `wc -l filename` to count the number of rows in a file
</details>

## 2 💪 Create a `venv`
- Deactivate your current `venv` to go back to system Python
- Navigate to your `~/Documents`
- Make a new directory called `sandbox`
- Navigate inside the `sandbox` directory
- Create a new environment using called `purple` using `venv` command
- Verify that the new environment is saved in the current directory
- Activate it
- Install the following packages: `numpy`, `requests`
- Verify the packages were installed
- Export the environment into a `requirements.txt`
- View the contents of `requirements.txt`
- Deactivate `purple` to go back to system Python
- Navigate back to `~/Desktop/python-intermediate-inflammation`


<details>
<summary>🔍 Click here for hints! </summary>

- Use `deactivate` to go back to system python
- Use `python3 -m venv name_of_environment` to create a new environment
- Use `source name_of_environment/bin/activate` activate the environment file
- Use `python3 -m pip install package` to install a package
- Use `python3 -m pip list` to see list of currently installed packages
- Use `python3 -m pip freeze > requirements.txt` to save the environment
</details>

## 3 💪 Requirements file

- Verify that `pytest` is appearing in your virtual environment
- Compare this list to the list of packages inside `requirements.txt`
- Regenerate the `requirements.txt` file
- Confirm that `pytest` is included in `requirements.txt` file 


<details>
<summary>🔍 Click here for hints! </summary>

- Use `python3 -m pip list` to see the list of packages in the current virtual environment
- Use `cat a_file.txt` to see the contents of the `a_file.txt`
- Use `python3 -m pip freeze > requirements.txt` to generate a new `requirements.txt` file
</details>

## 4 💪 Write some unit tests 
- Write a unit test for `daily_max()` in a similar format than the test `test_daily_mean_integers()`
- Modify `test_input` including to include negative values
- Modify `test_result` to match the current `test_input` 
- Once added, run all the tests again and you should also see your new test passing
- Make sure the `"""docstring"""` reflects the content of the test
- Once all tests pass, commit your changes to `git`

<details>
<summary>🔍 Click here for hints! </summary>

- Start by duplicating an existing test and adapting it to the new function you want to test
- Remember to import the new function to be tested at the top of `test_models.py` > `from inflammation.models import daily_max` 
- Remember that these functions take a 2D array and return a 1D array with each element the result of analysing each *column* of the data
- Use `pytest tests/test_models.py` or `python -m pytest tests/test_models.py` to run the tests again
</details>


#### 🚀 Optional challenge
Do the same for `daily_min`


## 5 💪 Add data validation unit tests 
- Write a data validation unit test for `daily_mean()`
- Once added, run all the tests again and you should also see your new test passing
- Make sure the `"""docstring"""` reflects the content of the test
- Once all tests pass, commit your changes to `git`

<details>
<summary>🔍 Click here for hints! </summary>

- Start by duplicating an existing test and adapting it to the new function you want to test
- Use `pytest tests/test_models.py` or `python -m pytest tests/test_models.py` to run the tests again
</details>


#### 🚀 Optional challenge
What happens when you input a non-iterable value (e.g. a single integer)? Write a test case for this.

<details>
<summary>🔍 Click here for hints! </summary>

- The error raised in this case is not `TypeError`. Adapt the test to use the error type (e.g. `AxisError`, `IndexError`)
</details>


## 6 💪 Improve Code Style of Our Project  

- Open `inflammation-analysis.py` from VSCode
- Go to `View > Command Palette`, or shortcut `CTRL-SHIFT-P` (Windows, Linux) or `CMD-SHIFT-P` (macOS)
- Search for `Ruff: format document` 
- Save `inflammation-analysis.py`
- Go to the `git view` to see the modifications that were done by the plugin
- Don't commit yet
- Go back to `inflammation-analysis.py`
- Check for naming conventions
- Save `inflammation-analysis.py`
- Commit your changes to `git`


## 7 💪 Fix the docstrings

- Look into `models.py` from VSCode
- Go to the function `daily_mean`
- Delete the current `docstring`
- Execute the `Generate docstring` plugin
- Replace the text `_summary_` with an explanation of *what* the function does
- Replace the text `_description_` with a description of the variables store, what is the expected shape and type
- Add type hints for the input and output parameters
- Commit your changes to `git`

<details>
<summary>🔍 Click here for hints! </summary>

- To generate the docstring:
    - Stand where you want the `docstring`
    - Go to `View > Command Palette`, or shortcut `CTRL-SHIFT-P` (Windows, Linux) or `CMD-SHIFT-P` (macOS)
    - Execute `Generate Docstring`

</details>
