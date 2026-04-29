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
- Write a unit test for `daily_max()` in a similar format than the test `test_daily_mean_integers()`
- Define `test_input` and `test_result` cases that are suitably different 
- Once added, run all the tests again and you should also see your new test passing
- Make sure the `"""docstring"""` reflects the content of the test
- Once all tests pass, commit your changes to `git`

<details>
<summary>🔍 Click here for hints! </summary>


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

- Use `pytest tests/test_models.py` or `python -m pytest tests/test_models.py` to run the tests again
</details>


#### 🚀 Optional challenge
What happens when you input a non-iterable value (e.g. a single integer)? Write a test case for this.

<details>
<summary>🔍 Click here for hints! </summary>

- The error type raise in this case is `IndexError`
</details>


## 6 💪 Parametrize unit tests 
- Rewrite your test function for `daily_max()` to be parameterized
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


## 7 💪 Improve Code Style of Our Project  

- Open `inflammation-analysis.py` from VSCode
- Fix everything that the linter is highlighting
- After fixing one thing, save the file so that the linter finds the next issue to fix
- Also check for naming conventions 
- Commit your changes to `git`
<details>
<summary>🔍 Click here for hints! </summary>

For long function calls consider using a `hanging indent` such that:
 ```bash
    the_first_line_starts_flush(
        all_subsequent_lines,
        are_indented_more,
    )
```

</details>

## 8 💪 Fix the docstrings

- Look into `models.py` from VSCode
- Go to the function `daily_mean`
- Delete the current `docstring`
- Execute the `Generate docstring` plugin
- Replace the text `_summary_` with an explanation of *what* the function does
- Replace the text `_description_` with a description of the variables store, what is the expected shape and type
- Do the same for `daily_min` and `daily_max`
- Commit your changes to `git`

<details>
<summary>🔍 Click here for hints! </summary>

- To generate the docstring:
    - Stand where you want the `docstring`
    - Go to `View > Command Palette`, or shortcut `CTRL-SHIFT-P` (Windows, Linux) or `CMD-SHIFT-P` (macOS)
    - Execute `Generate Docstring`

</details>

## 9 💪 Simulate a contribution from a colleague

- Add a new file inside the folder `inflammation` called `analysis.py`
- Copy the following code inside the `analysis.py` file
```bash
"""Module containing mechanism for calculating standard deviation between datasets.
"""

import glob
import os
import numpy as np

from inflammation import models, views


def analyse_data(data_dir):
    """Calculates the standard deviation by day between datasets.

    Gets all the inflammation data from CSV files within a directory,
    works out the mean inflammation value for each day across all datasets,
    then plots the graphs of standard deviation of these means."""
    data_file_paths = glob.glob(os.path.join(data_dir, 'inflammation*.csv'))
    if len(data_file_paths) == 0:
        raise ValueError(f"No inflammation data CSV files found in path {data_dir}")
    data = map(models.load_csv, data_file_paths)


    means_by_day = map(models.daily_mean, data)
    means_by_day_matrix = np.stack(list(means_by_day))

    daily_standard_deviation = np.std(means_by_day_matrix, axis=0)

    graph_data = {
        'standard deviation by day': daily_standard_deviation,
    }
    views.visualize(graph_data)

```
- Save the file and commit your changes to `git`

## 10 💪 Decouple Data Loading from Data Analysis
[TODO rephrase]
Modify `analysis.py` to separate out the data loading functionality from analyse_data() into a new function load_inflammation_data(), that returns a list of 2D NumPy arrays with inflammation data loaded from all inflammation CSV files found in a specified directory path. Then, change your analyse_data() function to make use of this new function instead.


## 11 💪 Use Classes to Abstract out Data Loading
[TODO rephrase]
Inside `analysis.py`, declare a new class `CSVDataSource` that contains the `load_inflammation_data()` function we wrote in the previous exercise as a method of this class. The directory path where to load the files from should be passed in the class’ constructor method. Finally, construct an instance of the class `CSVDataSource` outside the statistical analysis and pass it to `analyse_data()` function.


At the end of this exercise, the code in the `analyse_data()` function should look like:

```bash
def analyse_data(data_source):
    data = data_source.load_inflammation_data()
```
The controller code should look like:

```bash
data_source = CSVDataSource(os.path.dirname(infiles[0]))
analyse_data(data_source)
```

## 12 💪 Add an Additional DataSource
[TODO rephrase]
Create another class that supports loading patient data from JSON files, with the appropriate `load_inflammation_data()` method. Here is an example function that you can add to your `models.py` file to load observations from a JSON file:

```bash
def load_json(filename):
    """Load a numpy array from a JSON document.
    
    Expected format:
    [
      {
        "observations": [0, 1]
      },
      {
        "observations": [0, 2]
      }    
    ]
    :param filename: Filename of CSV to load
    """
    with open(filename, 'r', encoding='utf-8') as file:
        data_as_json = json.load(file)
        return [np.array(entry['observations']) for entry in data_as_json]
```
Finally, at run-time, construct an appropriate data source instance based on the file extension.



## 13 💪 Write Regression Tests
[TODO rephrase]
Modify the `analyse_data()` function not to plot a graph and return the data instead. Then, add a new test file called test_analysis.py in the tests folder and add a regression test to verify the current output of analyse_data(). We will use this test in the remainder of this section to verify the output `analyse_data()` is unchanged each time we refactor or change code in the future.

Start from the skeleton test code below:

```bash
from inflammation.analysis import analyse_data

def test_analyse_data():
    path = os.path.join( os.getcwd(), "../data")
    data_source = CSVDataSource(path)
    result = analyse_data(data_source)
    # TODO: add assert statement(s) to test the result value is as expected
```
Use `assert_array_almost_equal` from the `numpy.testing` library to compare arrays of floating point numbers.


<details>
<summary>🔍 Click here for hints! </summary>

When determining the correct return data result to use in tests, it may be helpful to assert the result equals some random made-up data, observe the test fail initially and then copy and paste the correct result into the test.
</details>


## 14 💪 Refactoring To Use a Pure Function

Refactor the `analyse_data()` function to delegate the data analysis to a new pure function `compute_standard_deviation_by_day()` and separate it from the impure code that handles the input and output. The pure function should take in the data, and return the analysis result, as follows:

```bash
def compute_standard_deviation_by_day(data):
    # TODO
    return daily_standard_deviation
```

#### 🚀 Optional challenge

Add tests for `compute_standard_deviation_by_day()` that check for situations when there is only one file with multiple rows, multiple files with one row, and any other cases you can think of that should be tested.

<details>
<summary>🔍 Click here for hints! </summary>

You might have thought of more tests, but we can easily extend the test by parametrizing with more inputs and expected outputs:
```bash
@pytest.mark.parametrize('data,expected_output', [
    ([[[0, 1, 0], [0, 2, 0]]], [0, 0, 0]),
    ([[[0, 2, 0]], [[0, 1, 0]]], [0, math.sqrt(0.25), 0]),
    ([[[0, 1, 0], [0, 2, 0]], [[0, 1, 0], [0, 2, 0]]], [0, 0, 0])
],
ids=['Two patients in same file', 'Two patients in different files', 'Two identical patients in two different files'])
def test_compute_standard_deviation_by_day(data, expected_output):
    from inflammation.analysis import compute_standard_deviation_by_day

    result = compute_standard_deviation_by_day(data)
    npt.assert_array_almost_equal(result, expected_output)

```
</details>

## 15 💪 Add optional input parameter
[TODO rephrase]
Add optional parameter:
  -- a filename for a figure. If paremeter exists, save figure to file insted of plot.show()
- git commit

## 16 💪 Organising code into modules
[TODO]