# Contributing to ansible-mariadb-galera-cluster

## Table Of Contents

[Code of Conduct](#code-of-conduct)
[Environment Setup](#environment-setup)
[Running Tests](#running-tests)

## Code of Conduct

This project and everyone participating in it is governed by the [ansible-mariadb-galera-cluster Code of Conduct](CODE_OF_CONDUCT.md). By participating, you are expected to uphold this code. Please report unacceptable behavior to [me@example.com](mailto:me@example.com).

## Environment Setup
```
python3.10 -m venv .venv
source .venv/bin/activate

# Install Requirements
pip install -r requirements.txt -r requirements-dev.txt
pip install pre-commit

# One-Time Install of Commit Hooks
pre-commit install
```

## Running Tests
This project uses [Molecule](https://molecule.readthedocs.io/en/latest/index.html) to run tests:
```
molecule test --scenario-name $SCENARIO
```

For a list of valid scenario names, see the folders listed under `molecule`.  To see the scenario names run as part of continuous integration testing, see `.github/workflows/default.yml`.
