
## 1 💪 Simulate a contribution from a colleague

- Add a new file inside the folder `inflammation` called `analysis.py`
- Copy the following code inside the `analysis.py` file
```bash
"""Module containing mechanism for calculating standard deviation between datasets.
"""

import glob
import os
import numpy as np
import argparse

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

if __name__ == '__main__':
    # Initialize the parser
    parser = argparse.ArgumentParser(
        description="Calculate standard deviation by day between datasets."
    )
    
    # Add the data_dir argument
    parser.add_argument(
        'data_dir', 
        type=str, 
        help="Path to the directory containing the inflammation CSV files."
    )
    
    # Parse the arguments from the command line
    args = parser.parse_args()
    
    # Run the function using the provided argument
    analyse_data(args.data_dir)
```
- Save the file and commit your changes to `git`


## 2 💪 Write Regression Tests

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


## 3 💪 Decouple Data Loading from Data Analysis

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

## 4 💪 Add a unit test for `Patient.get_patient_dosage`

A unit test is an excellent way to test that a new implementation works as expected. Let's test our new `Patient` class. (The dosages in this course are made up.)

- Open the file `test_patient.py`
- Add a `test_case` called `test_get_patient_dosage`
- Create an instance of the class `Patient` with the properties:
    - `name = 'maria'`
    - `weight = 60`
- Use `assert_almost_equal` from the `numpy.testing` library to compare the result of `get_patient_dosage(dose_per_kg=2.5)` to the theoretical value of `150.0`
- Save the file, then go back to the **Testing** panel and click **Run All Tests** again.
- All tests should be green ✅
- Commit your changes to `git`

<details>
<summary>🔍 Click here for hints! </summary>

- `get_patient_dosage` is a function so it requires that you use the parenthesis at the end when calling it, and that you give it a value for `dose_per_kg`, like this: `get_patient_dosage(dose_per_kg=2.5)` 
- Remember to import `numpy.testing as npt` at the top of the file. Then compare the values like `npt.assert_almost_equal(patient_instance.get_patient_dosage(dose_per_kg=2.5), expected_dosage)`
</details>

#### 🚀 Optional challenge

Make sure to pass the parameters to the class constructor using parameters name. Then swap the parameters around and confirm that the test still passes.

## 5 💪 Use Classes to Abstract out Data Loading

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


## 6 💪 Refactoring To Use a Pure Function

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

