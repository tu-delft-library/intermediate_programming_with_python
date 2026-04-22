# Exercises

## 1 💪 Have a peek at the data

- How many rows does `inflammation-01.csv` have?
- How many rows does `small-01.csv` have?

<details>
<summary>🔍 Click here for hints! </summary>history

- Use `wc -l filename` to count the number of rows in a file
</details>

## 2 💪 Create a `venv`
- Deactivate your current `venv` to go back to system Python
- Navigate to your `Desktop`
- Make a new directory called `sandbox`
- Navigate inside the `sandbox` directory
- Create a new environment using `venv`
- Verify that the new environment is saved in the current directory
- Activate it
- Install the following packages: `numpy`, `requests`
- Verify the packages were installed
- Export the environment into a `requirements.txt`
- View the contents of `requirements.txt`
- Deactivate the `venv` to go back to system Python
- Navigate back to `python-intermediate-inflammation`


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
- Use `python3 -m pip freeze --exclude-editable > requirements.txt` to generate a new `requirements.txt` file
</details>

## 4 💪 Write some unit tests 
- Write a unit test for `daily_min()` in a similar format than the test `test_daily_mean_integers()`
- Define `test_input` and `test_result` cases that are suitably different 
- Once added, run all the tests again and you should also see your new test passing
- Make sure the `"""docstring"""` reflects the content of the test
- Once all tests pass, commit your changes to `git`

<details>
<summary>🔍 Click here for hints! </summary>


- Remember to import the new function to be tested at the top of `test_models.py` > `from inflammation.models import daily_min` 
- Remember that these functions take a 2D array and return a 1D array with each element the result of analysing each *column* of the data
- Use `pytest tests/test_models.py` or `python -m pytest tests/test_models.py` to run the tests again
</details>


#### 🚀 Optional challenge
Do the same for `daily_max`


## 5 💪 Add data validation unit tests 
- Write a data validation unit test for `daily_mean()`
- Once added, run all the tests again and you should also see your new test passing
- Make sure the `"""docstring"""` reflects the content of the test
- Once all tests pass, commit your changes to `git`

<details>
<summary>🔍 Click here for hints! </summary>

- Use `pytest tests/test_models.py` or `python -m pytest tests/test_models.py` to run the tests again
</details>


#### 🚀 Optional challenge
What happens when you input a non-iterable value (e.g. a single integer)? Write a test case for this.

<details>
<summary>🔍 Click here for hints! </summary>

- The error type raise in this case is `IndexError`
</details>




## 6 💪 Parametrize unit tests 
- Rewrite your test function for `daily_min()` to be parameterized
- Add an extra set of `test_input, test_result`
- Once added, run all the tests again and you should also see your new test passing
- Make sure the `"""docstring"""` reflects the content of the test
- Once all tests pass, commit your changes to `git`

<details>
<summary>🔍 Click here for hints! </summary>

- Use `pytest tests/test_models.py` or `python -m pytest tests/test_models.py` to run the tests again
</details>


#### 🚀 Optional challenge
- What edge cases can you think of? Add them to the parametrize sets

## 7 💪 Python Coding Style Guide  
[TODO rephrase]

Modify `inflammation-analysis.py` from VSCode, which is helpfully marking inconsistencies with coding guidelines by underlying them. 

- Commit your changes to `git`

## 8 💪 Fix the docstrings
[TODO rephrase]
Look into `models.py` from VSCode and improve docstrings for functions `daily_mean , daily_min, daily_max`. 
- Commit your changes to `git`

## 9 💪 Decouple Data Loading from Data Analysis
[TODO rephrase]
Modify `compute_data.py` to separate out the data loading functionality from analyse_data() into a new function load_inflammation_data(), that returns a list of 2D NumPy arrays with inflammation data loaded from all inflammation CSV files found in a specified directory path. Then, change your analyse_data() function to make use of this new function instead.


## 9 💪 Use Classes to Abstract out Data Loading

Inside compute_data.py, declare a new class CSVDataSource that contains the load_inflammation_data() function we wrote in the previous exercise as a method of this class. The directory path where to load the files from should be passed in the class’ constructor method. Finally, construct an instance of the class CSVDataSource outside the statistical analysis and pass it to analyse_data() function.


At the end of this exercise, the code in the analyse_data() function should look like:

```bash
def analyse_data(data_source):
    data = data_source.load_inflammation_data()
```
The controller code should look like:

```bash
data_source = CSVDataSource(os.path.dirname(infiles[0]))
analyse_data(data_source)

<details>
<summary>🔍 Click here for hints! </summary>

- tip
</details>


#### 🚀 Optional challenge