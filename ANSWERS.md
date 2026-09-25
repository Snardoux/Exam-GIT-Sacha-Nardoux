# Evaluation report

Answer concisely in your own words. Refer to concrete evidence from the project
and its tools.

## 1. Initial assessment

What was incomplete, incorrectly configured, or failing when you first examined
the repository? Explain how you discovered each item.

Response : 

When I first examined the repository, there was no README and .gitignore file. The initial quality checks also revealed several problems. Running uv run pytest collected nine tests and produced three failures: click_through_rate returned 0.05 instead of the expected percentage 5.0, and count_campaign_tags returned 2 instead of 3. Running uv run mypy src reported that .strip() was called on a value typed as str | None. Finally, uv run ruff format --check . reported that metrics.py needed to be reformatted. 


## 2. Toolchain evidence

What did the quality chain tools contribute to
your investigation? Give relevant examples and distinguish the kinds of
problems they can detect.

Response : 

uv run pytest found 9 tests with 3 failed and 6 passes.
================================================ short test summary info ===================================================
FAILED tests/test_metrics.py::test_click_through_rate_returns_a_percentage - assert 0.05 == 5.0 ± 5.0e-06
FAILED tests/test_metrics.py::test_count_campaign_tags_returns_the_number_of_tags - AssertionError: assert 2 == 3
FAILED tests/test_metrics.py::test_normalize_campaign_name_accepts_none - AttributeError: 'NoneType' object has no attribute 'strip'
============================================== 3 failed, 6 passed in 0.16s ==================================================
uv run mypy src reported an unsafe .strip() call on a value typed as str | None.

uv run ruff format --check . reported that metrics.py needed formatting.
(base) sachanardoux@MacBook-Air-de-Sacha-N evaluation-python-test-main % uv run ruff format --check .
unformatted: File would be reformatted
 --> src/campaign_validator/metrics.py:1:1
  |
  -
  -
1 | def click_through_rate(clicks: int, impressions: int) -> float:
The initial Ruff configuration selected only the E, F, and I rule families

## 3. Corrections

Describe the implementation and configuration corrections you made. For each
important correction, connect the original problem, the evidence, and the
resulting behavior.

Response :

Changed the rate calculation to clicks / impressions * 100, producing 5.0.
Changed the tag counter to use len(tags) and not len(tags)-1.
Made normalize_campaign_name return an empty string when the input is None.
Reformatted metrics.py with Ruff.
The final result was 9 passed like mypy, Ruff linting, and Ruff formatting also passed.

## 4. Reproducibility and local configuration

Explain how the completed repository lets another developer reconstruct,
configure, run, and verify the project safely.

Response :

pyproject.toml declares the Python version and development tools.
uv.lock records the dependency version.
CAMPAIGN_ACCESS_TOKEN is put in the .env file.
The README explain installation, configuration and execution.

## 5. Git workflow

Explain how your branches and commits divide the work into reviewable changes.
Mention how the completed work was integrated.

Response :

The original files were saved in the initial commit with the name 'original files'.
The corrections were divided into 3 branches:
fix/format-metrics
fix/mypy-none-handling
fix/metrics-calculations
Each branch represents a different corrections.
The branches were the, merged into main.

## 6. Limits of verification

Why does a completely passing quality toolchain provide useful evidence but not
proof that the program contains no defects?

Response :

A passing toolchain shows that configured type rules, linting rules, and formatting checks currently pass. It does not prove that buisness expectation are fullfill or security issue are adress.



## 7. Bonus question

Document the investigation trail for the bonus question:

1. How did you decide which project tool was responsible for this type of
   policy?

   Response :

2. What documentation or repository evidence did you consult?

Response :

3. Which rule or rule family did you identify, and what behavior does it check?

Response :

4. What configuration did you change, and how did you verify that every existing
   rule remained enabled?

   Response :

5. What new diagnostic appeared after the configuration change?

Response :
