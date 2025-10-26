Testing seaborn locally

- Prerequisites
  - Python 3.8+ and dev tools.
  - Install test dependencies: `pip install -e .[dev]`.
  - Optional stats dependencies (for full coverage of regression tests): `pip install -e .[dev,stats]` or, for a non-editable install, `pip install seaborn[stats]`.

- Running tests
  - Run all tests with coverage: `make test`.
  - Lint the code: `make lint`.

- Dataset caching (to reduce/avoid network access)
  - Some tests exercise loading example datasets hosted at `mwaskom/seaborn-data` via `seaborn.utils.load_dataset`. The loader caches files under a user cache directory or a path specified by the `SEABORN_DATA` environment variable.
  - To pre-cache all example datasets for offline or repeatable test runs:

    1) Choose a cache directory and export `SEABORN_DATA`:

       ```bash
       export SEABORN_DATA="$PWD/.seaborn-data"
       ```

    2) Prefetch and cache all datasets:

       ```bash
       python - <<'PY'
       from seaborn.utils import get_dataset_names, load_dataset
       names = get_dataset_names()
       for name in names:
           load_dataset(name, cache=True)
       print(f"Cached {len(names)} datasets to SEABORN_DATA")
       PY
       ```

  - Notes:
    - Subsequent calls to `load_dataset(..., cache=True)` will use the cached files.
    - If the network is unavailable, tests decorated to require the network will be skipped; pre-caching minimizes reliance on live downloads during test runs.

- Optional statsmodels
  - Some regression-related functionality and tests depend on `statsmodels` (and `scipy`). If they are not installed, those tests will be skipped.
  - To include them, install the `stats` extra along with dev tools:
    - Editable: `pip install -e .[dev,stats]`
    - Non-editable: `pip install seaborn[stats]` (you will still need dev tools to run tests).
  - Quick check:
    - `python -c "import statsmodels, seaborn.utils as u; print('statsmodels OK'); print('cache dir:', u.get_data_home())"`

- References
  - Dataset caching logic: `seaborn/utils.py` (`get_data_home`, `load_dataset`).
  - Example dataset names: `seaborn.utils.get_dataset_names()`.

