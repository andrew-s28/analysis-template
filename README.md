# Scientific Analysis Code Template

This repository serves as a template for organizing and storing scientific analysis code in a structured and reproducible manner, with a particular focus on supporting Python notebook version control.

## Purpose

This template was primarily designed because I got sick of dealing with a million different Python notebooks, none of which were being version controlled (who hasn't had `analysis-updated-new_data_v3.ipynb`?). By providing a template directory structure and commit workflow, future analyses will:
- Maintain organized scientific analysis code
- Promote reproducibility in research
- Include version control for Python notebooks
- Simplify dependency management
- Encourage clean, consistent code

## Getting Started

To use this template:

1. Click the "Use this template" button in GitHub and select "Create a new repository".
2. Clone the created repository onto your local machine.
3. Add your analysis code and commit changes, following the instructions below.

## Committing Python Notebooks

Finding a good solution to version controlling Python notebooks was the primary motivation for making this template. The workflow used here depends on using [pre-commit](https://pre-commit.com/) with a [jupytext hook](https://jupytext.readthedocs.io/en/latest/using-pre-commit.html), along with [ruff](https://docs.astral.sh/ruff/) for linting and formatting the produced Python files.

Follow these steps for version controlling your Python notebooks in this repository:

1. Create Python notebooks in the `notebooks/` directory (such as `notebooks/example.ipynb`).
2. `pre-commit install`

   install pre-commit hooks (if pre-commit is not installed, try `uv tool install pre-commit`)

3. `git add notebooks/example.ipynb && git commit -m 'adds new notebook'`

   this command should fail, but generates a Python file using [jupytext](https://jupytext.readthedocs.io/en/latest/index.html).

4. `git add notebooks/example.ipynb notebooks/python/example.py && git commit -m 'adds new notebook'`

   add and commit new Python file and updated notebook, this time the command should succeed. Note you may want to double check the files after step 2 to ensure they were generated and formatted correctly.

5. Make changes to `notebooks/example.ipynb` and repeat from step 2 (with a new commit message!).

Though it should work the same, it is recommended not to edit Python scripts in `notebooks/python/` directly. If you need to use Python scripts (for example, for writing shared functions), place these in the `scripts/` directory

## Managing Dependencies with uv

In order to run your Python notebooks, you will need some third-party libraries. `uv` is an excellent tool for managing these dependencies. For more details on installation and usage, see the [uv docs](https://docs.astral.sh/uv/). Note that the `pyproject.toml` is setup to require `python>=3.11`, so make sure to change that if you have a dependency that requires an older Python version.

1. `uv add numpy`

   adds `numpy` dependency to `pyproject.toml` (make sure to commit the new `pyproject.toml` and `uv.lock` files!)

2. `uv sync`

   initialize a `.venv` with the dependencies from `pyproject.toml` installed (from here it should be synced automatically)

3. `.venv/bin/activate` (Bash) or `.\.venv\Scripts\activate` (Powershell)

   activates virtual environment

Some packages with non-Python dependencies may require conda environment management. In that case, don't use `uv`, instead prefer:

1. `conda create -n analysis python=3.12 pip && conda activate analysis`
   
   create conda environment with specific Python version

2. `conda install numpy`
   
   install `numpy` dependency to `analysis` environment

3. `conda env export > environment.yml`
   
   save dependencies once environment is setup (make sure to commit the new `environment.yml` file!)

## Directory Structure

```
analysis-template/
├── data/                  # Raw and processed data (consider using .gitignore for large files)
├── notebooks/             # Primary analysis Python notebooks
│   └── python/            # Python scripts generated by jupytext for version control
├── scripts/               # Independent Python scripts (not converted)
├── manuscript/            # Figures for manuscripts
├── presentation/          # Figures for presentations
├── misc/                  # Miscellaneous files
├── pyproject.toml         # Project specifications and dependency control (recommend using uv)
└── README.md              # Project overview and instructions
```

Directories are initialized with a `.placeholder` file so they can be committed to version control "empty". You can remove these files when other files are added to the directory or ignore them entirely.

## Contributions

If you think something could be improved, open an issue - I would love to discuss better ways to manage analysis code! Though do keep in mind that this repository was made primarily for my own use cases, so if we disagree, I might recommend cloning it and making changes in your local repository.

## License

This template is licensed under the [MIT License](./LICENSE). Please give credit if you find this template useful :).
