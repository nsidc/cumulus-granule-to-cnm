# cumulus-granule-to-cnm

This is the Python code for the lambda `cumulus-granule-to-cnm`. It takes in a list of Granules and converts each one to a CNM (Cloud Notification Mechanism) and returns said list.

## Getting Started

### Dependencies

* [Poetry](https://python-poetry.org/docs/#installing-with-the-official-installer)
* Optional: [pyenv](https://github.com/pyenv/pyenv) and [pyenv-virtualenv](https://github.com/pyenv/pyenv-virtualenv)

### Installing

* Option 1: Use `pyenv` to create a virtual environment and activate it

        $ pyenv virtualenv 3.11.9 cumulus-granule-to-cnm
        $ pyenv activate cumulus-granule-to-cnm
        $ poetry shell

* Option 2: Use Poetry to create and activate a virtual environment

        $ poetry shell

* Install dependencies

        $ poetry install

### Build

        $ sh ./build.sh

### Run Tests

        $ poetry run pytest --cov cumulus_granule_to_cnm tests -v

### Manual Build

This command creates the `dist/` folder:

        $ poetry build

This command downloads the dependency files from the just created .whl file, along with the lambda_handler function in `cumulus_granule_to_cnm/cumulus_granule_to_cnm.py`, and places them in the `package` folder:

        $ poetry run pip install --upgrade -t package dist/*.whl

Note that **boto3** and **moto** are not being pulled in. I've specifically excluded them via having them as `tool.poetry.dev-dependencies` only (via the `pyproject.toml` file)

The last command used is:

        $ cd package ; zip -r ../artifact.zip . -x '*.pyc'

Which zips and creates the `artifact.zip` file, containing all files found in `package/` excluding .pyc files


### Executing program

Upload `artifact.zip` to any location you plan to use it

* Upload directly as a lambda with `aws lambda update-function-code`
* Upload to your AWS S3 `lambdas/` folder so that your Cumulus Terraform Build can use it

NOTE: This AWS Lambda Function is intended to be used as a cumulus task and this requires `provider` and `provider_path` inputs from the `task_config`:

    "task_config": {
        "provider": "{$.meta.provider}",
        "provider_path": "{$.meta.provider_path}"
    }

## Help

`This <https://chariotsolutions.com/blog/post/building-lambdas-with-poetry/>`_ page contains some good info on the overall "building a zip with poetry that's compatible with AWS Lambda".

## Version History

See the [CHANGELOG](CHANGELOG.md)

## License

TBD

## Acknowledgments

Forked from PODAAC's [cumulus-granule-to-cnm](https://github.com/podaac/cumulus-granule-to-cnm) GitHub repo
