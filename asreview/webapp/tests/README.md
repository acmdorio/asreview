# ASReview Test Suite

This folder contains the test suite of the ASReview app. It is organized in the following folders:

* __config__: Contains data to create user accounts and a number of json files to start the ASReview app in different modes (e.g. authenticated, authentication with verification, etc).

* __data__: Contains project data used in tests.

* __integration_tests__: Forthcoming.

* __test_api__: Contains API related tests. These tests are independent, unit-like tests.

* __test_database_and_models__: Contains tests that focus on basic database operations and several SQL_Alchemy models that are used in the authenticated version of the ASReview app.

* __test_extensions__: Forthcoming.

* __utils__: Contains various helper files that facilitate writing tests.

## Requirements

1. [Pytest](https://docs.pytest.org/)
2. [Pytest-random-order](https://github.com/jbasko/pytest-random-order)

## Fixtures

If you are unfamiliar with Pytest: fixtures enable a setup and teardown mechanism for all tests. They ensure a controlled initial state and clean up afterwards to make sure the next test is conducted with a clean slate. 

In this suite, all fixtures are defined in `conftest.py` files. These fixtures are automatically picked up by Pytest and can be found in the `/webapp/tests`-folder, but also in nested folders. The nested conftest modules use fixtures defined in the tests-folder without actually importing them: Pytest finds the required fixture if you use their function-name as parameter in either a newly defined fixture or test function.

Note that the `conftest.py` in the `/webapp/tests` contains an important fixture `asreview_path_fixture`. This suite uses an alternative ASReview folder to avoid disrupting the data of the app in production or development mode. The `asreview_path_fixture` fixture ensures that for every test the ASReview app always starts with its own dedicated, temporary, ASReview folder. Pytest states that these folders are automatically removed (https://docs.pytest.org/en/7.1.x/how-to/tmp_path.html#the-default-base-temporary-directory). This might be unwanted behavior if checking the state of the ASReview folder is required. If so, simply create your own ASReview folder in the `asreview_path_fixture` fixture.

## Test functions

Ideally a test function tests one particular feature and can be executed independently from other test functions. If you would like to add a feature or an enhancement to the ASReview app, please accompany your code with appropriate tests. Append your tests to existing modules, or create a new module. If you are adding routes to the API, please wrap your test API calls in a function and place it in `/utils/api_utils.py`. If applicable, make sure your feature works both in an authenticated and an unauthenticated environment.

## Running the tests

__Important__: if you run the entire test stuite, please make sure you have compiles the app's assets:
```
python setup.py compile_assets
```

Please run your tests with the `--random-order` option to ensure test independency. With Pytest you can run all tests within a particular module. For example (running from the asreview root-folder):

```
pytest --random-order -s -v ./asreview/webapp/tests/test_api/test_projects.py
```

The `-s` option enables capturing stdout (shows your print statements if there are any), and the `-v` option makes the output more verbose. Run an entire folder containing test modules like so:

```
pytest --random-order -s -v ./asreview/webapp/tests/test_api/
```

If you are in the middle of writing your tests, and your module contains many tests, it is more efficient to run a small cluster or a single test. One of the many possibilities is the `-k` option that executes only tests with a function name that ends with a certain postfix. In the next example we execute only test functions that end with the 'current` postfix in the `test_projects.py` module:

```
pytest --random-order -s -v ./asreview/webapp/tests/test_api/test_projects.py -k current
```