---
layout: page
title: "Code coverage"
permalink: /developer/coverage
nav_order: 4
parent: Developer information
---

{% include toc.html %}

The "Code coverage" value of a codebase indicates how much of the production/development code is covered by the running unit tests. Maintainers try their best to keep this percentage high, and this process is often automated using tools such as `GitHub Actions` and `Codecov`. Hence, code coverage becomes a good metric (not always) to check if a particular codebase is well tested and reliable.

Tools and libraries used to calculate, read, and visualize coverage reports:

- `pytest-cov`: allows developers to calculate and visualize the coverage value
- `Codecov`: integrates with remote repositories, allowing developers to see and compare coverage value with each CI run
- `GitHub Actions`: allows users to automatically upload coverage reports to `Codecov`

> ### Are there any alternatives?
>
> `coverage.py` is a popular Python library used to calculate coverage values, but it is usually paired with python's `unittest`. On the other hand, `pytest-cov` is built to integrate with `pytest` with minimal extra configurations. Further, Coveralls is an alternative coverage platform, but we recommend using Codecov because of its ease of use and integration with GitHub Actions.

> ### Should increasing the coverage value be my top priority?
>
> A low coverage percentage will definitely motivate you to add more tests, but adding weak tests just for coverage's sake is not a good idea. The tests should test your codebase thoroughly and should not be unreliable.

### Calculating code coverage locally

`pytest` allows users to pass the `--cov` option to automatically invoke `pytest-cov`, which then generates a `.coverage` file with the calculated coverage value. The value of `--cov` option should be set to the name of your package. For example, run the following command to run tests to generate a `.coverage` file for the vector package -

```
python -m pytest -ra --cov=vector tests/
```

`--cov` option will also print a minimal coverage report in the terminal itself! See the [docs](https://pytest-cov.readthedocs.io/en/latest/) for more options.

### Calculating code coverage in your workflows

Your workflows should also run tests with the `--cov` option, which must be set to your package name. For example -

```yaml
- name: Test package
  run: python -m pytest -ra --cov=vector tests/
```

This will automatically invoke `pytest-cov`, and generate a `.coverage` file, which can then be uploaded to `Codecov` using the [codecov/codecov-action][] action.

### Configuring Codecov and uploading coverage reports

Interestingly, `Codecov` does not require any initial configurations for your project, given that you have already signed up for the same using your GitHub account. `Codecov` requires you to push or upload your coverage report, after which it automatically generates a `Codecov` project for you.

All `Scikit-HEP` packages using `Codecov`, with detailed coverage reports and graphs, are available at [https://app.codecov.io/gh/scikit-hep](https://app.codecov.io/gh/scikit-hep). After your workflows push your first coverage report to `Codecov` your repository should also appear here.

`Codecov` maintains the [codecov/codecov-action][] GitHub Action to make uploading coverage reports easy for users. A minimal working example for uploading coverage reports through your workflow, which should be more than enough for a simple testing suite, can be written as follows:

```yaml
- name: Upload coverage report
  uses: codecov/codecov-action@v3.1.0
```

The lines above should be added after the step that runs your tests with the `--cov` option. See the [docs](https://github.com/codecov/codecov-action#usage) for all the optional options.

### Using codecov.yml

One can also configure `Codecov` and coverage reports passed to `Codecov` using `codecov.yml`. `codecov.yml` should be placed inside the `.github` folder, along with your `workflows` folder. Additionally, `Codecov` allows you to create and edit this `YAML` file directly through your `Codecov` project's settings!

A recommended configuration for `Codecov`:

```yaml
codecov:
  notify:
    after_n_builds: x
```

where `x` is the number of uploaded reports `Codecov` should wait to receive before sending statuses. This would ensure that the `Codecov` checks don't fail before all the coverage reports are uploaded. See the [docs](https://docs.codecov.com/docs/codecov-yaml) for all the options.

<!-- ### Coverage for projects written in Python and C++

TODO -->

[codecov/codecov-action]: https://github.com/codecov/codecov-action