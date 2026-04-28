# PRACTICAL: Catching Bugs


## Learning Objectives

By the end of this lab you will be able to:

- Recognise how a failing test reveals a bug in production code
- Use the VSCode debugger to inspect a program's state at the point of failure
- Fix a bug and verify the whole test suite passes again
- *(Optional)* Write new test cases for edge cases


## Part 1 — Introduce a Bug

You are going to deliberately break `daily_min` so that a test catches it. This simulates the kind of copy-paste mistake that is easy to make in real life.

Open `models.py` and change `daily_min` so computes the minimum along axis 1:

```python
# BEFORE (correct)
def daily_min(data):
    """Calculate the daily min of a 2d inflammation data array."""
    return np.min(data, axis=0)

# AFTER (buggy — change this line)
def daily_min(data):
    """Calculate the daily min of a 2d inflammation data array."""
    return np.min(data, axis=1)   # <-- bug introduced here
```

Save the file

## Part 2 — Run the Tests and Watch One Fail

Open the **Testing** panel in VSCode by clicking the flask icon (⚗️) in the left Activity Bar, or via the menu **View → Testing**.

If this is your first time, VSCode will ask you to configure a test framework — select **pytest** and point it at your `tests/` folder. Once configured, you will see your test files and individual test functions listed in a tree.

Click **Run All Tests** (the ▶▶ button at the top of the Testing panel). After a few seconds, the tree will update:

- ✅ Green circles = passing tests  
- ❌ Red circles = failing tests

You should see `test_daily_min` marked with a red ❌ on the second parametrized case. Click on that failing test to expand it and read the error in the panel below:

```
AssertionError: Arrays are not equal

Mismatched elements: 3 / 3 (100%)
Max absolute difference: 15
 ACTUAL: array([-1, -2, -9])
 DESIRED: array([1, -9, -1])
```

**Answer these questions before moving on:**

1. Which parametrized test case failed? (first or second?)
2. Why did the first test case (`all zeros`) still pass even with the bug present?


## Part 3 — Use the Debugger to Locate the Bug

Rather than just reading the error message, you will step through the code with the VSCode debugger to see *exactly* what is happening at runtime.

### Set a breakpoint in the editor

Open `models.py`. Click in the **gutter** (the narrow strip just to the left of the line numbers) on the `return` line inside `daily_min`. A red dot will appear — this is your `breakpoint 🔴`.

```python
def daily_min(data):
    """Calculate the daily min of a 2d inflammation data array."""
    return np.min(data, axis=1)   # 🔴 click the gutter here
```

### Run the failing test in Debug mode

In the **Testing** panel, find the second parametrized case of `test_daily_min` (the one with the ❌). Hover over it and click on **Debug Test** ▶️  + 🐞.

VSCode will switch to the **Run and Debug** view and pause execution on the breakpoint line. The editor highlights the current line in yellow.

### Inspect variables in the Debug sidebar

Look at the **Variables** panel on the left. Expand **Locals** to see `data` and its current value — this is the array that was passed in by the failing test case.

Then use the **Debug Console** (bottom panel, tab labelled *Debug Console*) to evaluate expressions interactively:

| Type this in the Debug Console | What you will see |
|-------------------------------|-------------------|
| `data` | The full 2D input array |
| `np.min(data, axis=0)` | What the result *should* be |
| `np.min(data, axis=1)` | What the function is *actually* returning |

Compare the two results. You should be able to see clearly that `np.min(data, axis=1)` is returning the wrong values for this test case.

In the `CALL STACK` click on `test_daily_min` to verify what is the expected value of `test_result`. Then click again on `daily_min` and confirm that `np.min(data, axis=0)` gives the expected value.


### Remove the breakpoint

Press **Stop** once you have confirmed the bug.

Click the red dot in the gutter again to remove it before moving on.


## Part 4 — Fix the Bug and Re-run the Suite

Restore the correct implementation:

```python
def daily_min(data):
    """Calculate the daily min of a 2d inflammation data array."""
    return np.min(data, axis=0)   # fixed
```

Save the file, then go back to the **Testing** panel and click **Run All Tests** again.

All tests should be green. ✅



## Part 5 — Edge Case Tests

The current test suite does not check a couple of important edge cases. Add the following parametrized cases to the **existing** `@pytest.mark.parametrize` block for `test_daily_min` in `test_models.py`:

### Edge case 1 — Array containing zeros

```python
([[0, 1, 2], [0, 3, 4]], [0, 1, 2]),
```

Use `Step Over` to see in `VARIABLES` the `(return)` value of `daily_min` before `npt.assert_array_equal` is executed.

Visually confirm that the expected value actually matches the expected return stored in `test_result`


**What to check:** Does `daily_min` return `0` correctly when zeros are present, rather than accidentally treating zero as "no data"?

### Edge case 2 — All values the same

```python
([[3, 3, 3], [3, 3, 3], [3, 3, 3]], [3, 3, 3]),
```

Use `Step Over` to see in `VARIABLES` the `(return)` value of `daily_min` before `npt.assert_array_equal` is executed.

Visually confirm that the expected value actually matches the expected return stored in `test_result`


After adding both cases, go to the **Testing** panel and click **Run All Tests** again to confirm they pass.



## 🚀  Optional Challenge

Add edge cases for `daily_max`

## Key point

The workflow you just followed — **red → debug → green** — is the core loop of test-driven debugging and something you will use throughout your career.
