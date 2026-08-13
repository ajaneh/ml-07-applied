# ml-07-applied

[![Workflow Guide](https://img.shields.io/badge/Pro--Guide-pro--analytics--02-green)](https://denisecase.github.io/pro-analytics-02/workflow-b-apply-example-project/)
[![Python 3.14](https://img.shields.io/badge/python-3.14%2B-blue?logo=python)](./pyproject.toml)
[![MIT](<https://img.shields.io/badge/license-see%20LICENSE-yellow.svg>)](./LICENSE)

> Professional Python project: investigating a deployed machine learning model.

## Project Description

This project focuses on learning to interrogate a deployed ML model
by probing it systematically with different inputs.

We learn to:

- call a live prediction API from a notebook
- vary input features and observe how predictions change
- identify decision boundaries and edge cases
- interpret model behavior from the outside

## Example Notebook + Your Notebook

Keep the example notebook as it is.
Either copy it or use it to build a new notebook that ends in _yourname.
See [docs/your-files.md] for more.

Links:

- [ml_07_case.ipynb](notebooks/ml_07_case.ipynb)
- [Custom Project](notebooks/ml_07_alex.ipynb)



## Command Reference

<details>
<summary>Show command reference</summary>

### In a machine terminal (open in your `Repos` folder)

After you get a copy of this repo in your own GitHub account,
open a machine terminal in your `Repos` folder:

```shell

git clone https://github.com/ajaneh/ml-07-applied

cd ml-07-applied
code .
```

### In a VS Code terminal

These are listed for convenience.
For best results, follow the detailed instructions in
[pro-analytics-02 guide](https://denisecase.github.io/pro-analytics-02/).

```shell
uv self update
uv python pin 3.14
uv lock --upgrade
uv sync --extra dev --extra docs --upgrade

uvx pre-commit install
uvx pre-commit autoupdate

git add -A
uvx pre-commit run --all-files
# repeat if changes were made
uvx pre-commit run --all-files

# run the example module to verify the environment (.venv/)
uv run python -m mlstudio.app_case
cd notebooks
open ml_07_alex
select kernal
run all

# run common chores
uv run ruff format .
uv run ruff check . --fix
uv run python -m pyright
uv run python -m pytest
uv run python -m zensical build

# save progress
git add -A
git commit -m "update"
git push -u origin main
```

</details>

## Findings and Visuals

This investigation probed the ML Penguin Predictor to understand how it classifies penguin species from biometric measurements. By systematically varying features and visualizing decision boundaries, I identified that **bill_length is the dominant predictor**, with a clear transition zone around 45mm separating Adelie from Chinstrap/Gentoo species. Other features contribute less in isolation, but combining bill_length, flipper_length, and bill_depth in 3D space reveals the full decision surface.

**Key finding**: The model reliably predicts species based on bill geometry, but lacks input validation—accepting physically impossible measurements without warning.

### Charts

![flipper_length_mm sensitivity sweep](./docs/images/Screenshot%202026-08-13%2016.03.53.png)

![bill_length_mm sensitivity sweepf](./docs/images/Screenshot%202026-08-13%2016.04.06.png)

![2D prediction grid: bill_length vs body_mass](./docs/images/Screenshot%202026-08-13%2016.04.36.png)

**Interactive 3D Plot** (click to explore in full interactivity):
[View 3D Decision Surface (fixed body_mass)](./docs/images/fixed_body_mass_3d_graph.html)



## Project Documentation

Additional project instructions, terms, and notes:

[docs/index.md](docs/index.md)

## Citation

[CITATION.cff](./CITATION.cff)

## License

[MIT](./LICENSE)
