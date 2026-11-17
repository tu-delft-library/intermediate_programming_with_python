# HOMEWORK 2

**≈ 60-75 minutes · Part A is required · the rest is optional practice**

| Part | What | Time | |
|---|---|---|---|
| **A** | **Get ready for Day 3** | 15' | **required** |
| B | Add an additional data source (JSON) | 20' | optional |
| C | Test the pure function directly | 10' | optional |
| D | Extra tests for `compute_standard_deviation_by_day` | 15' | optional 🚀 |
| E | Add a `--patient` command-line argument | 20' | optional 🚀 |

Day 3 starts from the same code for everybody, so you won't fall behind if you skip B to E. Commit to git after each part. ✅

## A. Get ready for Day 3

Tomorrow's work happens on GitHub, together with a team. These steps make sure you can start right away.

- [ ] **Run the tests** in your project (`python -m pytest`). Everything should be green ✅. If it isn't, don't worry: we have a fallback. Bring the question to Day 3
- [ ] **Push your work** to your fork on GitHub: `git status` should say there is nothing to commit, then `git push`. Open your fork on github.com and check that your latest commit is there
- [ ] **Share your GitHub username** with us in this form: **[FORM LINK]**, before **[DATE]**
- [ ] **Accept the invitation** to the GitHub organization **[ORG NAME]**. You will get an email from GitHub after you shared your username. Do this *before* Day 3: invitations take a moment to arrive and they can expire
- [ ] **Check that you can connect to GitHub from the terminal**: run `ssh -T git@github.com`. You should see a message starting with `Hi <your-username>!`. If you see an error, ask us before Day 3
- [ ] 📖 *(optional, 10')* Read GitHub's page [About pull requests](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests)

## B. Add an additional data source

In class you created `CSVDataSource` to separate loading data from analysing it. Now add a second data source for JSON files.

- Open `models.py`
- Add this function which loads observations from a JSON file:

```python
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
    :param filename: Filename of JSON to load
    """
    with open(filename, 'r', encoding='utf-8') as file:
        data_as_json = json.load(file)
        return [np.array(entry['observations']) for entry in data_as_json]
```
- Open `analysis.py`
- Duplicate the `CSVDataSource` class and rename it `JSONDataSource`
- Adapt `JSONDataSource` to load `JSON` files
- Modify the docstring accordingly
- Open `inflammation-analysis.py`
- Replace the line that creates an instance of `CSVDataSource` with an `if .. elif` logic that constructs an appropriate data source instance based on the file extension
- Run all tests and commit

<details>
<summary>🔍 Click here for hints! </summary>

- The function `load_json` requires to load the module `json` at the top of `models.py`
- The `load_inflammation_data` method in `JSONDataSource` should filter files by `"inflammation*.json"`
- Use `_, extension = os.path.splitext(infiles[0])` to split the file extension. Then use the variable `extension` in the `if .. elif` statement to choose the data source class
</details>

<details>
<summary>🔍 Click here for the solution </summary>

```python
class JSONDataSource:
  """
  Loads patient data with inflammation values from JSON files within a specified folder.
  """
  def __init__(self, dir_path):
    self.dir_path = dir_path

  def load_inflammation_data(self):
    data_file_paths = glob.glob(os.path.join(self.dir_path, 'inflammation*.json'))
    if len(data_file_paths) == 0:
      raise ValueError(f"No inflammation JSON files found in path {self.dir_path}")
    data = map(models.load_json, data_file_paths)
    return list(data)
```

In `inflammation-analysis.py`:

```python
    _, extension = os.path.splitext(infiles[0])
    if extension == '.json':
        data_source = analysis.JSONDataSource(os.path.dirname(infiles[0]))
    elif extension == '.csv':
        data_source = analysis.CSVDataSource(os.path.dirname(infiles[0]))
    else:
        raise ValueError(f'Unsupported data file format: {extension}')
    analysis.analyse_data(data_source)
```
</details>

## C. Test the pure function directly

Because `compute_dosage` is now a module-level function, you can write a focused unit test for it that does not require constructing a `Patient`. Add this to `test_patient.py`:

```python
from inflammation.models import Patient, compute_dosage

@pytest.mark.parametrize("weight, dose_per_kg, expected_dosage", [
    (70,   2.5, 175.0),
    (50,   0.5, 25.0),
    (82.5, 1.2, 99.0),
])
def test_compute_dosage_pure(weight, dose_per_kg, expected_dosage):
    """Test that compute_dosage returns the correct value for various inputs."""
    npt.assert_almost_equal(compute_dosage(weight, dose_per_kg), expected_dosage)
```

Make sure `npt` is imported at the top of the file (`import numpy.testing as npt`).

Save the file, then go back to the **Testing** panel and run `test_patient.py`

All tests should be green. ✅

> 🔖 **Commit your work to git**

## D. 🚀 Optional challenge: more tests for `compute_standard_deviation_by_day`

Add tests for `compute_standard_deviation_by_day()` that check for situations when there is only one file with multiple rows, multiple files with one row, and any other cases you can think of that should be tested.

<details>
<summary>🔍 Click here for hints! </summary>

You might have thought of more tests, but we can easily extend the test by parametrizing with more inputs and expected outputs:

```python
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

## E. 🚀 Optional challenge: add a `--patient` command-line argument

The `inflammation-analysis.py` script currently processes CSV files and saves figures. You will now add a new optional argument `--patient` that, when provided, prints a brief summary of one patient's data to the terminal.

#### Step 1 — Add the argument to the parser

Open `inflammation-analysis.py`. In the `if __name__ == "__main__":` block, add a new optional argument after the existing `"-outdir"` argument:

```python
parser.add_argument(
    "-patient",
    type=int,
    help="Row index (0-based) of the patient to summarise",
)
```

#### Step 2 — Add a helper function

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

#### Step 3 — Call the helper function from `main()`

Inside `main()`, after loading `inflammation_data`, add:

```python
if args.patient is not None:
    summarise_patient(inflammation_data, args.patient)
```

#### Step 4 — Test it from the terminal

```bash
python inflammation-analysis.py data/inflammation-01.csv -outdir data -patient 0
```

You should see a line like:

```
Patient 0: mean=5.45, max=18, min=0
```

Save the file, then go back to the **Testing** panel and click **Run All Tests**.

All tests should be green. ✅

> 🔖 **Commit your work to git**
