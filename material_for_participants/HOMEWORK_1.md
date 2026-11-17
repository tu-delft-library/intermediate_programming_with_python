# HOMEWORK 1

**≈ 60 minutes · optional but recommended · not needed for Day 2**

Day 2 starts from the same code for everybody, so you won't fall behind if you skip this. But the practice sticks much better if you do it while Day 1 is still fresh. Questions? Write them down and bring them to the start of Day 2.

| Part | What | Time |
|---|---|---|
| A | Parametrize unit tests (Exercise 6) | 15' |
| B | Finish the docstrings and type hints | 10' |
| C | The rest of the *Catching Bugs* practical (Parts 6-9) | 35' |

Remember: commit to git after each part. ✅

## A. Parametrize unit tests

- Rewrite your test function for `daily_max()` to be parametrized
- Add an extra set of `test_input, test_result`
- Once added, run all the tests again and you should also see your new test passing
- Make sure the `"""docstring"""` reflects the content of the test
- Once all tests pass, commit your changes to `git`

<details>
<summary>🔍 Click here for hints! </summary>

- Start by duplicating an existing test with parametrized input (for example `test_daily_mean`) and adapting it to the new function you want to test
- Use `pytest tests/test_models.py` or `python -m pytest tests/test_models.py` to run the tests again
</details>

<details>
<summary>🔍 Click here for the solution </summary>

```python
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
</details>

#### 🚀 Optional challenge

- What edge cases can you think of? Add them to the parametrize sets

## B. Finish the docstrings and type hints

In class you rewrote the docstring of `daily_mean`. Now do the same for the other two functions.

- Open `models.py`
- Go to the functions `daily_min` and `daily_max`
- Delete the current `docstring` and generate a new one with the `Generate Docstring` plugin
- Replace `_summary_` with an explanation of *what* the function does
- Replace `_description_` with a description of the variables: what is the expected shape and type?
- Add type hints for the input and output parameters
- Run the tests, then commit your changes to `git`

<details>
<summary>🔍 Click here for the solution </summary>

```python
def daily_max(data: np.ndarray) -> np.ndarray:
   """Calculate the daily maximum of a 2D inflammation data array for each day.

   :param data: A 2D data array with inflammation data (each row contains measurements for a single patient across all days).
   :return: An array of max values of measurements for each day.
   """
   return np.max(data, axis=0)


def daily_min(data: np.ndarray) -> np.ndarray:
   """Calculate the daily minimum of a 2D inflammation data array for each day.

   :param data: A 2D data array with inflammation data (each row contains measurements for a single patient across all days).
   :return: An array of minimum values of measurements for each day.
   """
   return np.min(data, axis=0)
```
</details>

## C. The rest of the *Catching Bugs* practical

In class you did Parts 1-5 of `PRACTICAL_unit_testing_debugging.md`: add a test, introduce a bug, watch the test fail, debug and fix it. Here are the remaining four parts.

### C1 — Edge case tests

The current test suite does not check a couple of important edge cases. Add the following parametrized cases to the **existing** `@pytest.mark.parametrize` block for `test_daily_min` in `test_models.py`:

```python
([[0, 1, 2], [0, 3, 4]], [0, 1, 2]),     # array containing zeros
([[3, 3, 3], [3, 3, 3], [3, 3, 3]], [3, 3, 3]), # all values the same
```

There should still be a `breakpoint 🔴` inside the function `daily_min` in the module `models.py`. If not, add it again by clicking in the **gutter** (the narrow strip just to the left of the line numbers) on the `return` line inside `daily_min`.

In the **Testing** panel, find test `test_daily_min`. Hover over it and click on **Debug Test** ▶️  + 🐞.


Use `Step Over (F10)` to see in `VARIABLES` the `(return)` value of `daily_min` before `npt.assert_array_equal` is executed.

Visually confirm that the expected value actually matches the expected return stored in `test_result`. Keep pressing `Step Over (F10)` to go through all the test cases.

When the tests stop running, All tests should be green ✅


> 🔖 **Commit your work to git**


### C2 — Data validation with `pytest.raises`

A good test suite checks not just that functions return the right answer for valid input, but also that they fail in the expected way for *invalid* input. This is called **data validation testing**.

The test suite already contains `test_daily_max_string`, which confirms that passing a list of strings raises a `TypeError`.

Now add a new data validation test for `daily_max`. An empty array cannot be reduced along an axis, which causes an `ValueError`. Add the following test to `test_models.py`:

```python
def test_daily_max_empty_array():
    """Test that daily_max raises ValueError when given an empty array."""
    with pytest.raises(ValueError):
        daily_max([])
```

Save the file, then go back to the Testing panel and click Run All Tests again.

All tests should be green. ✅

### C3 — Data validation with `NaN`
Another very useful test uses `NaN` values in the input array. The current behavior of the function `daily_max` is to select `np.nan` as the maximum value. Add the test below to the `test_models.py` file.

```python
def test_daily_max_nan_propagation():
    data = np.array([[1, np.nan], [3, 4]])
    result = daily_max(data)
    assert np.isnan(result[1])  # documents current behavior
```
Add a breakpoint on the line `result = daily_max(data)`. Run the test in Debug mode: in the **Testing** panel, find test `test_daily_max_nan_propagation`. Hover over it and click on **Debug Test** ▶️  + 🐞.

Use `Step Over (F10)` to see in `VARIABLES` the `result` before `assert np.isnan` is executed. Stop the debugger

Run the full test suite and confirm everything is green ✅

> 🔖 **Commit your work to git**


### C4 — Check code style

#### Ruff linter

The Ruff linter checks that code follows the PEP 8 style guide. It does *not* check whether your code is correct — that is what tests are for — but consistent style makes code easier to read and review.

With the file `test_models.py` open in the editor, open the Command Palette (`View > Command Palette`, or shortcut `CTRL-SHIFT-P` (Windows, Linux) or `CMD-SHIFT-P` (macOS))

Search for `Ruff: format document` and enter to execute. Save the file.

Go to the `git view` to see the modifications that were done by the plugin.

Check any other warnings underlined in yellow by the Ruff plugin. Fix them if possible.

> 🔖 **Commit your work to git**

#### Docstrings

Revise all the `docstrings` in `test_models.py` to make sure they correctly reflect what each test is doing.

Run the full test suite one more time to confirm that your style fixes have not accidentally broken anything. ✅

> 🔖 **Commit your work to git**

### 🚀 Optional challenge

Do C1, C2, C3 and C4 for `daily_max`
