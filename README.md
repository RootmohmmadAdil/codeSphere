
1) Create & switch to the branch
```bash
git checkout -b fix/zero-division
```

2) Create/update files (use these exact contents)

README.md
```markdown
# student-result-calculator

## Overview
A small Python CLI to calculate students' average marks.

## Bug report: Program crashes when input value is zero

### Project Context
- **Project Name:** student-result-calculator
- **Language:** Python
- **Module:** calculator.py

### Bug Description
When the user enters `0` as input for total subjects, the program crashes with a `ZeroDivisionError` instead of showing a proper error message.

### Buggy Code (example)
```python
def calculate_average(total_marks, total_subjects):
    average = total_marks / total_subjects
    return average

marks = int(input("Enter total marks: "))
subjects = int(input("Enter total subjects: "))

result = calculate_average(marks, subjects)
print("Average marks:", result)
```

### Steps to Reproduce
1. Run the program
2. Enter:
```
Enter total marks: 500
Enter total subjects: 0
```
3. Program crashes

### Actual Result
```
ZeroDivisionError: division by zero
```

### Expected Result
Program should display:
```
Error: Total subjects cannot be zero
```

### Suggested Fix
Validate input and show a user-friendly error. See `calculator.py`.

### Bug Type
- Runtime Error
- Logical Validation Bug

### Interview Explanation (One Line)
This bug occurred due to missing input validation, causing a division by zero runtime error.

---

## Files added/updated
- `calculator.py` — input validation and friendly error messages
- `tests/test_calculator.py` — pytest tests for the function
- `CONTRIBUTING.md` — quick guide to run tests and contribute

## How to run
1. Install dependencies (none required beyond Python and pytest for tests).
2. Run the calculator:
```bash
python calculator.py
```
3. Run tests:
```bash
python -m pytest
```
```

calculator.py
```python
"""
calculator.py

Simple CLI to calculate average marks with input validation.
"""

def calculate_average(total_marks: int, total_subjects: int) -> float:
    """
    Calculate average marks.

    Raises:
        ValueError: If total_subjects is zero or negative.
    """
    if total_subjects == 0:
        raise ValueError("Total subjects cannot be zero")
    if total_subjects < 0:
        raise ValueError("Total subjects cannot be negative")
    return total_marks / total_subjects


def main():
    try:
        marks_input = input("Enter total marks: ").strip()
        subjects_input = input("Enter total subjects: ").strip()

        marks = int(marks_input)
        subjects = int(subjects_input)
    except ValueError:
        print("Error: Please enter valid integers for marks and subjects.")
        return

    try:
        average = calculate_average(marks, subjects)
    except ValueError as exc:
        print(f"Error: {exc}")
    else:
        print("Average marks:", average)


if __name__ == "__main__":
    main()
```

tests/test_calculator.py
```python
import pytest
from calculator import calculate_average

def test_calculate_average_valid():
    assert calculate_average(500, 5) == pytest.approx(100.0)

def test_calculate_average_zero_subjects():
    with pytest.raises(ValueError) as exc:
        calculate_average(500, 0)
    assert "Total subjects cannot be zero" in str(exc.value)

def test_calculate_average_negative_subjects():
    with pytest.raises(ValueError) as exc:
        calculate_average(500, -2)
    assert "Total subjects cannot be negative" in str(exc.value)
```

CONTRIBUTING.md
```markdown
# Contributing

Thanks for contributing!

## Run the CLI
```bash
python calculator.py
```

## Run tests
This project uses pytest. From the repository root:
```bash
python -m pytest
```

## Style and tests
- Keep changes small and focused.
- Add tests for bug fixes and new behavior.

## Branching
I suggest a branch name like `fix/zero-division` for this fix.

## Publishing an issue
I can publish the drafted issue describing the bug and link it to the branch/PR if you want.
```

3) Add, commit, push
```bash
git add README.md calculator.py tests/test_calculator.py CONTRIBUTING.md
git commit -m "fix: handle zero/invalid subjects in calculator; add tests and docs"
git push -u origin fix/zero-division
```

4) Open a pull request (web UI or gh CLI)
Using the GitHub CLI:
```bash
gh pr create --title "Fix: handle zero subjects in calculator" \
  --body "Validate total subjects to avoid ZeroDivisionError; add tests and CONTRIBUTING. Fixes #<issue-number-if-published>" \
  --base main
```

5) Publish the drafted issue (optional)
You can publish the issue via web UI (paste the issue body you approved) or via gh CLI:
```bash
gh issue create --title "Program crashes on zero subjects input in calculator.py (ZeroDivisionError)" \
  --body "$(cat ISSUE_BODY.md)" --label bug
```
(Replace ISSUE_BODY.md with a file containing the issue content.)

6) Run tests locally to confirm
```bash
python -m pytest -q
```



Would you like me to push and publish the draft issue now (I will use branch fix/zero-division), or do you want to run the commands yourself in your Codespace?

Copilot is powered by AI, so mistakes are possible. Leave a comment via the 👍 👎 to share your feedback and help improve the experience.
