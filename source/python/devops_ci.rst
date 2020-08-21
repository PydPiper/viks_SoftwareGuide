DevOps - Continuous Integration (CI)
====================================
Continuous Integration (CI) is used for various Development Operations (DevOps), such as running
unittests, integration tests, generating code monitor reports like coverage, and lint for PEP8 code
quality, and much more. In this section, we will focus on automating our testing with CI on ``circleci``
and utilizing code coverage report hosting on ``codecov``. There are many options out there for CI hosts,
but I found these to be the easiest to use.

0.) Navigate to `circleci <https://circleci.com/>`_ and `codecov <https://codecov.io/>`_ and connect your
    github account with these APIs.

1.) Create a ``.circleci`` folder in your top level repo directory

2.) Create a ``config.yml`` file within the ``.circleci`` folder and paste the following in it:

.. code-block::

    # definition of circleCI version
    version: 2.1
    orbs:
      # defines the connection to codecov's API
      codecov: codecov/codecov@1.1.1


    # define workflow of executions
    workflows:
      version: 2.1
      # name your workflow
      test:
        # define the job names under test
        jobs:
          - test27
          - test38

    jobs:
      test27:
        docker:
        # create a python image on the server
          - image: circleci/python:2.7

        # change server dir to repo where we clone down the repo
        working_directory: ~/repo

        steps:
          # download repo
          - checkout

          # install dependencies
          - run:
              name: install dependencies
              # note on linux virtualenv is in venv/bin/activate not venv/Scripts/activate
              command: |
                python -m pip install virtualenv
                python -m virtualenv venv
                . venv/bin/activate
                python -m pip install pytest pytest-cov

          # run tests
          - run:
              name: run tests
              # note you have to reactive on each -run
              command: |
                . venv/bin/activate
                python -m pytest

      test38:
        docker:
          - image: circleci/python:3.8

        # change server dir to repo where we clone down the repo
        working_directory: ~/repo

        steps:
          # download repo
          - checkout

          # install dependencies
          - run:
              name: install dependencies
              # note on linux virtualenv is in venv/bin/activate not venv/Scripts/activate
              command: |
                python -m pip install virtualenv
                python -m virtualenv venv
                . venv/bin/activate
                python -m pip install pytest pytest-cov

          # run tests
          - run:
              name: run tests
              # note you have to reactive on each -run
              command: |
                . venv/bin/activate
                python -m pytest --cov-report=xml --cov='.'
                ls

        # point codecov to your coverage file and upload it to their API
          - codecov/upload:
              file: coverage.xml