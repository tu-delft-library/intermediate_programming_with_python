# PRACTICAL: Extending the Patient Model

## Learning Objectives

By the end of this lab you will be able to:

- Add attributes and methods to an existing class
- Write unit tests for class methods, including data validation
- Extract a pure function from a class method and test it independently
- Confirm your test suite stays green after every change
- Commit working code at each checkpoint


## The Starting Point

The codebase now has a `Patient` class in `models.py` with three attributes (`name`, `weight`, `height`) and one method (`get_body_mass_index`).

Here is the relevant part of `models.py` as it currently stands:

```python
class Patient:
    def __init__(self, name: str, weight: float, height: float):
        """Patient class

        :param name: Name of patient
        :param weight: Weight in kilograms
        :param height: Height in meters
        """
        self.name = name
        self.weight = weight
        self.height = height

    def get_body_mass_index(self):
        """Compute body mass index: (weight in kg) / (height in meters)**2
        """
        return self.weight / self.height ** 2
```

---

## Part 1 — Add a `is_overweight` Method

A BMI above 25 is generally considered overweight. Add a method to `Patient` that returns `True` if the patient is overweight and `False` otherwise.

Open `models.py` and add the following method inside the `Patient` class (keep the same indentation level as `get_body_mass_index`):

```python
def is_overweight(self):
    """Return True if patient BMI is above 25, False otherwise."""
    return self.get_body_mass_index() > 25
```

Save the file, then go back to the **Testing** panel and click **Run All Tests**.

All tests should be green. ✅

> 🔖 **Commit your work to git**


## Part 2 — Write Tests for `is_overweight`

Open `tests/test_patient.py` and add the following parametrized test below the existing tests. Notice how we use `pytest.mark.parametrize` to cover several cases with a single test function:

```python
@pytest.mark.parametrize("name, weight, height, expected", [
    ("Alice",  80, 1.7,  True),   # BMI ≈ 27.7 → overweight
    ("Bob",    60, 1.8,  False),  # BMI ≈ 18.5 → not overweight
    ("Carol",  68, 1.65, True),   # BMI ≈ 25.0 → not overweight (boundary)
    ("David",  67, 1.65, False),  # BMI ≈ 24.6 → not overweight
])
def test_is_overweight(name, weight, height, expected):
    """Test that is_overweight returns the correct boolean for various BMI values."""
    patient = Patient(name=name, weight=weight, height=height)
    assert patient.is_overweight() == expected
```

Make sure to add `import pytest` at the top of `test_patient.py` if it is not already there, and import `Patient` from `inflammation.models`.

**Before running the tests**, look at the third case: Carol has a BMI of exactly 25.0. The method uses a strict `>` comparison, so BMI = 25.0 should return `False`. Check that `expected` in that row is consistent with that.

Save the file, then go back to the **Testing** panel and run `test_patient.py`

All tests should be green. ✅

> 🔖 **Commit your work to git**


## Part 3 — Add Data Validation to the Patient Constructor

Right now, nothing prevents a user from creating a patient with nonsensical values:

```python
Patient(name="Oops", weight=-10, height=0)   # would cause ZeroDivisionError later
```

Failures like this are much easier to diagnose if we catch them early and raise a meaningful error. This is called *failing early*.

Add input validation inside `__init__`. Raise a `ValueError` if:

- `weight` is less than or equal to zero
- `height` is less than or equal to zero

The constructor should look like this after the change:

```python
def __init__(self, name: str, weight: float, height: float):
    """Patient class

    :param name: Name of patient
    :param weight: Weight in kilograms
    :param height: Height in meters
    :raises ValueError: If weight or height are not positive numbers.
    """
    if weight <= 0:
        raise ValueError("weight must be a positive number")
    if height <= 0:
        raise ValueError("height must be a positive number")
    self.name = name
    self.weight = weight
    self.height = height
```

Save the file, then go back to the **Testing** panel and click **Run All Tests**.

All tests should be green. ✅

> 🔖 **Commit your work to git**


## Part 4 — Test the Data Validation

Now write tests that confirm the `ValueError` is raised when it should be. Add these to `test_patient.py`:

```python
def test_patient_negative_weight():
    """Test that creating a patient with non-positive weight raises ValueError."""
    with pytest.raises(ValueError):
        Patient(name="Invalid", weight=-5, height=1.7)


def test_patient_zero_height():
    """Test that creating a patient with zero height raises ValueError."""
    with pytest.raises(ValueError):
        Patient(name="Invalid", weight=70, height=0)
```

Save the file, then go back to the **Testing** panel and click **Run All Tests**.

All tests should be green. ✅

> 🔖 **Commit your work to git**


## Part 5 — Extract a Pure Function for BMI Calculation

The `get_body_mass_index` method works correctly, but the formula itself — `weight / height ** 2` — is a standalone piece of logic that does not depend on any other part of the `Patient` class. Making it a module-level function means it can be:

- tested without constructing a `Patient`
- reused elsewhere in the codebase

Add the following **outside** the class (at module level, after the class definition):

```python
def compute_bmi(weight: float, height: float) -> float:
    """Calculate body mass index from weight and height.

    :param weight: Weight in kilograms
    :param height: Height in meters
    :return: Body mass index (kg/m²)
    """
    return weight / height ** 2
```

Now update `get_body_mass_index` inside the class to call this function instead of repeating the formula:

```python
def get_body_mass_index(self):
    """Compute body mass index using weight and height attributes."""
    return compute_bmi(self.weight, self.height)
```

Save the file, then go back to the **Testing** panel and click **Run All Tests**.

All tests should be green. ✅

> 🔖 **Commit your work to git**


## Part 6 — Test the Pure Function Directly

Because `compute_bmi` is now a module-level function, you can write a focused unit test for it that does not require constructing a `Patient`. Add this to `test_patient.py`:

```python
from inflammation.models import Patient, compute_bmi

@pytest.mark.parametrize("weight, height, expected_bmi", [
    (70,  1.75, 22.857142857142858),
    (90,  1.80, 27.777777777777779),
    (50,  1.60, 19.531250),
])
def test_compute_bmi(weight, height, expected_bmi):
    """Test that compute_bmi returns the correct value for various inputs."""
    npt.assert_almost_equal(compute_bmi(weight, height), expected_bmi)
```

Make sure `npt` is imported at the top of the file (`import numpy.testing as npt`).

Save the file, then go back to the **Testing** panel and run `test_patient.py`

All tests should be green. ✅

> 🔖 **Commit your work to git**


## 🚀 Optional Challenge — Add a `--patient` Command-Line Argument

The `inflammation-analysis.py` script currently processes CSV files and saves figures. You will now add a new optional argument `--patient` that, when provided, prints a brief summary of one patient's data to the terminal.

### Step 1 — Add the argument to the parser

Open `inflammation-analysis.py`. In the `if __name__ == "__main__":` block, add a new optional argument after the existing `"-outdir"` argument:

```python
parser.add_argument(
    "--patient",
    type=int,
    help="Row index (0-based) of the patient to summarise",
)
```

### Step 2 — Add a helper function

Add a small pure function **above** `main()` in `inflammation-analysis.py`:

```python
def summarise_patient(inflammation_data, patient_index):
    """Print a summary of a single patient's inflammation data.

    :param inflammation_data: 2D NumPy array of inflammation readings
    :param patient_index: Row index of the patient to summarise
    """
    row = inflammation_data[patient_index]
    print(f"Patient {patient_index}: "
          f"mean={row.mean():.2f}, "
          f"max={row.max():.0f}, "
          f"min={row.min():.0f}")
```

### Step 3 — Call the helper from `main()`

Inside `main()`, after loading `inflammation_data`, add:

```python
if args.patient is not None:
    summarise_patient(inflammation_data, args.patient)
```

### Step 4 — Test it from the terminal

```bash
python inflammation-analysis.py data/inflammation-01.csv -outdir data --patient 0
```

You should see a line like:

```
Patient 0: mean=5.45, max=18, min=0
```

Save the file, then go back to the **Testing** panel and click **Run All Tests**.

All tests should be green. ✅

> 🔖 **Commit your work to git**



