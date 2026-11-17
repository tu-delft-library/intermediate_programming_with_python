# PRACTICAL: Extending the Patient Model

## Learning Objectives

By the end of this lab you will be able to:

- Make a function accept an optional argument, so a command-line option works
- Add attributes and methods to an existing class
- Write unit tests for class methods, including data validation
- Extract a pure function from a class method
- Confirm your test suite stays green after every change
- Commit working code at each checkpoint


## Part 1 — Fix the `-outdir` Option

At the end of the last session, `inflammation-analysis.py` started passing an output file name to `views.visualize()`, but `visualize` does not accept it yet. The script is broken. Let's fix it.

- Open `views.py`
- Modify `visualize` to receive another argument `outfile`
- Use an `if ... else` statement to either save the figure to `outfile` or show the figure
- Run the script from the integrated terminal in different conditions:
    - single file, no `-outdir`
    - single file, `-outdir data`
    - multiple input files, no `-outdir`
    - multiple input files, `-outdir data`
- Check that the `.png` files were saved in the `data` folder
- Run all tests: they should be green ✅

<details>
<summary>🔍 Click here for hints! </summary>

- Use `plt.savefig(outfile)` to save the file to disk
- If the figure is still showing, move `fig.tight_layout()` inside the `else: .... plt.show()` block
</details>

> 🔖 **Commit your work to git**


## The Starting Point for Parts 2-6

The codebase now has a `Patient` class in `models.py` with two attributes (`name`, `weight`) and one method (`get_patient_dosage`). *(All dosages in this practical are made up for teaching. They are not medical guidance.)*

Here is the relevant part of `models.py` as it currently stands:

```python
class Patient:
    def __init__(self, name: str, weight: float):
        """Patient class

        :param name: Name of patient
        :param weight: Weight in kilograms
        """
        self.name = name
        self.weight = weight

    def get_patient_dosage(self, dose_per_kg):
        """Compute dosage in mg: (weight in kg) * (dose in mg per kg)
        """
        return self.weight * dose_per_kg
```

---

## Part 2 — Add an `exceeds_max_dose` Method

A medicine often has a **maximum dose**. Add a method to `Patient` that returns `True` if the patient's dosage is **above** a given maximum, and `False` otherwise.

Open `models.py` and add the following method inside the `Patient` class (keep the same indentation level as `get_patient_dosage`):

```python
def exceeds_max_dose(self, dose_per_kg, max_dose):
    """Return True if the dosage in mg is above max_dose, False otherwise."""
    return self.get_patient_dosage(dose_per_kg) > max_dose
```

Notice that the new method *reuses* `get_patient_dosage` instead of repeating the formula.

Save the file, then go back to the **Testing** panel and click **Run All Tests**.

All tests should be green. ✅

> 🔖 **Commit your work to git**


## Part 3 — Write Tests for `exceeds_max_dose`

Open `tests/test_patient.py` and add the following parametrized test below the existing tests. Notice how we use `pytest.mark.parametrize` to cover several cases with a single test function:

```python
@pytest.mark.parametrize("name, weight, dose_per_kg, max_dose, expected", [
    ("Alice",  80, 5.0, 350, True),    # 400 mg → above the limit
    ("Bob",    60, 5.0, 350, False),   # 300 mg → below the limit
    ("Carol",  70, 5.0, 350, False),   # 350 mg → exactly at the limit (boundary): not above
    ("David",  71, 5.0, 350, True),    # 355 mg → just above the limit
])
def test_exceeds_max_dose(name, weight, dose_per_kg, max_dose, expected):
    """Test that exceeds_max_dose returns the correct boolean around the limit."""
    patient = Patient(name=name, weight=weight)
    assert patient.exceeds_max_dose(dose_per_kg, max_dose) == expected
```

Make sure to add `import pytest` at the top of `test_patient.py` if it is not already there, and import `Patient` from `inflammation.models`.

**Before running the tests**, look at the Carol and David rows: Carol's dosage is *exactly* the maximum and David's is just above it. The method uses a strict `>` comparison, so a dosage equal to the limit should return `False`. Check that `expected` in each row is consistent with that.

Save the file, then go back to the **Testing** panel and run `test_patient.py`

All tests should be green. ✅

> 🔖 **Commit your work to git**


## Part 4 — Add Data Validation

Right now, nothing prevents a user from creating nonsensical values:

```python
Patient(name="Oops", weight=-10)                                  # a negative weight
Patient(name="Oops", weight=70).get_patient_dosage(dose_per_kg=-2)  # silently gives -140 mg
```

Failures like this are much easier to diagnose if we catch them early and raise a meaningful error. This is called *failing early*.

Add input validation in two places. Raise a `ValueError` if:

- `weight` is less than or equal to zero (in `__init__`)
- `dose_per_kg` is less than or equal to zero (in `get_patient_dosage`)

The constructor and the method should look like this after the change:

```python
def __init__(self, name: str, weight: float):
    """Patient class

    :param name: Name of patient
    :param weight: Weight in kilograms
    :raises ValueError: If weight is not a positive number.
    """
    if weight <= 0:
        raise ValueError("weight must be a positive number")
    self.name = name
    self.weight = weight

def get_patient_dosage(self, dose_per_kg):
    """Compute dosage in mg: (weight in kg) * (dose in mg per kg)

    :param dose_per_kg: Prescribed dose in mg per kg of body weight
    :return: Dosage in mg
    :raises ValueError: If dose_per_kg is not a positive number.
    """
    if dose_per_kg <= 0:
        raise ValueError("dose_per_kg must be a positive number")
    return self.weight * dose_per_kg
```

Save the file, then go back to the **Testing** panel and click **Run All Tests**.

All tests should be green. ✅

> 🔖 **Commit your work to git**


## Part 5 — Test the Data Validation

Now write tests that confirm the `ValueError` is raised when it should be. Add these to `test_patient.py`:

```python
def test_patient_negative_weight():
    """Test that creating a patient with non-positive weight raises ValueError."""
    with pytest.raises(ValueError):
        Patient(name="Invalid", weight=-5)


def test_dosage_zero_dose_per_kg():
    """Test that a non-positive dose per kg raises ValueError."""
    patient = Patient(name="Valid", weight=70)
    with pytest.raises(ValueError):
        patient.get_patient_dosage(dose_per_kg=0)
```

Save the file, then go back to the **Testing** panel and click **Run All Tests**.

All tests should be green. ✅

> 🔖 **Commit your work to git**


## Part 6 — Extract a Pure Function for the Dosage Calculation

The `get_patient_dosage` method works correctly, but the formula itself — `weight * dose_per_kg` — is a standalone piece of logic that does not depend on any other part of the `Patient` class. Making it a module-level function means it can be:

- tested without constructing a `Patient`
- reused elsewhere in the codebase

Add the following **outside** the class (at module level, after the class definition):

```python
def compute_dosage(weight: float, dose_per_kg: float) -> float:
    """Calculate a dosage in mg from weight and prescribed dose per kg.

    :param weight: Weight in kilograms
    :param dose_per_kg: Prescribed dose in mg per kg of body weight
    :return: Dosage in mg
    """
    return weight * dose_per_kg
```

Now update the last line of `get_patient_dosage` inside the class to call this function instead of repeating the formula (the validation stays where it is):

```python
def get_patient_dosage(self, dose_per_kg):
    """Compute dosage in mg using the weight attribute and the prescribed dose per kg.

    :param dose_per_kg: Prescribed dose in mg per kg of body weight
    :return: Dosage in mg
    :raises ValueError: If dose_per_kg is not a positive number.
    """
    if dose_per_kg <= 0:
        raise ValueError("dose_per_kg must be a positive number")
    return compute_dosage(self.weight, dose_per_kg)
```

Save the file, then go back to the **Testing** panel and click **Run All Tests**.

All tests should be green. ✅

> 🔖 **Commit your work to git**


> 🏠 **HOME ASSIGNMENT 2** continues from here: testing `compute_dosage` directly, and an optional challenge to add a `--patient` command-line argument. See `HOME_ASSIGNMENT_2.md`.
