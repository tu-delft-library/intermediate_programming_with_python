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
[TODO]
- Write unit tests for daily_min(), daily_max()
- git commit

<details>
<summary>🔍 Click here for hints! </summary>

- tip
</details>



## XX 💪 


<details>
<summary>🔍 Click here for hints! </summary>

- tip
</details>


#### 🚀 Optional challenge