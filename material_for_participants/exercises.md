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
- Navigate to your `~/Desktop`
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


## 6 💪 Parametrize unit tests 
- Rewrite your test function for `daily_max()` to be parameterized
- Add an extra set of `test_input, test_result`
- Once added, run all the tests again and you should also see your new test passing
- Make sure the `"""docstring"""` reflects the content of the test
- Once all tests pass, commit your changes to `git`

<details>
<summary>🔍 Click here for hints! </summary>

- Start by duplicating an existing test with parametrized input and adapting it to the new function you want to test
- Use `pytest tests/test_models.py` or `python -m pytest tests/test_models.py` to run the tests again
</details>

#### 🚀 Optional challenge
- What edge cases can you think of? Add them to the parametrize sets


## 7 💪 Improve Code Style of Our Project  

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


## 8 💪 Fix the docstrings

- Look into `models.py` from VSCode
- Go to the function `daily_mean`
- Delete the current `docstring`
- Execute the `Generate docstring` plugin
- Replace the text `_summary_` with an explanation of *what* the function does
- Replace the text `_description_` with a description of the variables store, what is the expected shape and type
- Do the same for `daily_min` and `daily_max`
- Add type hints for the input and output parameters
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


## 10 💪 Write Regression Tests

- Open `analysis.py`
- Modify the `analyse_data()` function not to plot a graph and return the `daily_standard_deviation` instead. 
- Add a new test file called `test_analysis.py` in the tests folder from the skeleton test code below:

```bash
from inflammation.analysis import analyse_data

def test_analyse_data():
    path = os.path.join( os.getcwd(), "data")
    result = analyse_data(path)
    print(result) # TODO: replace print with assert statement(s) to test the result value is as expected
```
- Add a breakpoint on the line `print(result)`
- In the **Testing** panel, find test `test_analyse_data`. Hover over it and click on **Debug Test** ▶️  + 🐞.
- To copy the current result you have two options:
    - **Safest** Inspect the value of the array `result` in the `VARIABLES` panel on the left. You can copy it from there. 
    - **Less safe** Continue running the test and copy was is printed on the `Debug Console`. This option is less safe because the printing may be truncated and not show every value.
- Store the array copied from the previous step in a variable called `expected`
- Replace the `print(result)` statement with an `assert_array_almost_equal` from the `numpy.testing` library to compare of floating point numbers.
- Once all tests pass, commit your changes to `git`

<details>
<summary>🔍 Click here for hints! </summary>

- Remember to `import` the package `os` at the top of the `test_analyse_data`
- Remember to `import` the package `numpy.testing as npt` at the top of the `test_analyse_data`
- If the test fails because it doesn't find the path, inspect the value of `path` in the `Debug Console` and modify the line `path = os.path.join( os.getcwd(), "data")` until it matches the path with the csv files: e.g. `~/Desktop/python-intermediate-inflammation/data`
</details>


## 11 💪 Decouple Data Loading from Data Analysis

- Open `analysis.py` 
- Extract the data loading functionality from `analyse_data` into a new function `load_inflammation_data()` that returns a list of 2D NumPy arrays with inflammation data loaded from all inflammation CSV files found in a specified directory path. 
- Change your `analyse_data()` function to make use of this new function instead.
- Save the file, then go back to the **Testing** panel and click **Run All Tests** again.
- All tests should be green ✅
- Commit your changes to `git`

<details>
<summary>🔍 Click here for hints! </summary>

- Make sure the parameter `data_dir` is passed to the new function `load_inflammation_data()`
- Return the results of `data = map(models.load_csv, data_file_paths)` as a list like this:
`return list(data)`
</details>

## 12 💪 Add a unit test for `Patient.get_body_mass_index`
A unit test is an excellent way to test that a new implementation works as expected. Let's test our new `Patient` class.

- Open the file `test_patient.py`
- Add a `test_case` called `test_compute_bmi`
- Create an instance of the class `Patient` with the properties:
    - `name = 'maria'`
    - `weight = 60`
    - `height = 1.6`
- Use `assert_almost_equal` from the `numpy.testing` library to compare the results to the theoretical value of `23.4375`
- Save the file, then go back to the **Testing** panel and click **Run All Tests** again.
- All tests should be green ✅
- Commit your changes to `git`

<details>
<summary>🔍 Click here for hints! </summary>

- `get_body_mass_index` is a function so it requires that you use the parenthesis at the end when calling it, like this: `get_body_mass_index()` 
- Remember to import `numpy.testing as npt` at the top of the file. Then compare the values like `npt.assert_almost_equal(patient_instance.get_body_mass_index(), expected_bmi)`
</details>

#### 🚀 Optional challenge

Make sure to pass the parameters to the class constructor using parameters name. Then swap the parameters around and confirm that the test still passes.

## 13 💪 Use Classes to Abstract out Data Loading

- Open `analysis.py`
- At the TOP of the file, declare a new class `CSVDataSource`
- Define a constructor that takes the directory path where to load the files from
- Move the function `load_inflammation_data()` inside this new class
- Modify `analyse_data` to use this class to load files from `data_dir`

At the end of this exercise, the code in the `analyse_data()` function should look like:

```bash
def analyse_data(data_dir):
    data_source = CSVDataSource(data_dir)
    data = data_source.load_inflammation_data()
```
- Save the file, then go back to the **Testing** panel and click **Run All Tests** again.
- All tests should be green ✅
- Commit your changes to `git`

<details>
<summary>🔍 Click here for hints! </summary>

- Once the method `load_inflammation_data` is inside the class `CSVDataSource`, use `self.data_dir` inside the `glob` function call to collect the `data_file_paths`

- `load_inflammation_data` is a function so it requires that you use the parenthesis at the end when calling it, like this: `load_inflammation_data()` 

</details>

## 15 💪 Add an Additional DataSource

- Open `models.py`
- Add this function which load observations from a JSON file:

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
- Open `analysis.py`
- Duplicate the `CSVDataSource` class and rename as `JSONDataSource`
- Adapt `JSONDataSource` to load `JSON` files
- Modify the docstring accordingly
- Open `inflammation-analysis.py`
- Replace the line that creates an instance of `CSVDataSource` with an `if .. elif` logic that constructs an appropriate data source instance based on the file extension


<details>
<summary>🔍 Click here for hints! </summary>

- The function `load_json` requires to load the module `json` at the top of `models.py`
- The `load_inflammation_data` method in `JSONDataSource` should filter files by `"inflammation*.json"`
- Use `  _, extension = os.path.splitext(in_files[0])` to split the file extension. Then use the variable `extension` in the `if .. elif` statement to choose the data source class
</details>

## 16 💪 Refactoring To Use a Pure Function

- Open `analysis.py`
- Refactor the `analyse_data()` function to delegate the data analysis to a new pure function `compute_standard_deviation_by_day()`
- The pure function should take in the data, and return the analysis result, as follows:

```bash
def compute_standard_deviation_by_day(data):
    ...
    return daily_standard_deviation
```
- Save the file, then go back to the **Testing** panel and click **Run All Tests** again.
- All tests should be green ✅
- Commit your changes to `git`


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

## 17 💪 Use optional input parameter to save figures

- Open `views.py`
- Modify `visualize` to receive another argument `outfile`
- Use an `if ... else` statement to either save the figure to `outfile` or show the figure 
- Run the script from the integrated terminal in different conditions:
    - single file, no `-outdir`
    - single file, `-outdir data`
    - with multiple input files, no `-outdir`
    - multiple input files,  `-outdir data`
- Check that the `.png` files were saved in the `data` folder
- Commit your changes to `git`

<details>
<summary>🔍 Click here for hints! </summary>

- Use `plt.savefig(outfile)` to save the file to disk
- If the figure is still showing, move `fig.tight_layout()` inside the `else: .... plt.show()` block
</details>