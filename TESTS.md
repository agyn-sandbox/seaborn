TESTS

Prerequisites
- Python >= 3.10
- pip
- Dependencies: numpy, pandas, matplotlib, pytest, pytest-xdist, pytest-cov; optional statsmodels

Setup
- pip install -e .
- pip install numpy pandas matplotlib pytest pytest-xdist pytest-cov

Run tests
- pytest -n auto --cov=seaborn --cov=tests tests

Troubleshooting
- Network-dependent tests may fail offline; skip as needed.
- Install statsmodels to run corresponding tests.

