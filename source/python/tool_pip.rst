tool - pip (Package Installer for Python)
=========================================

General
-------

- To show list of packages available: ``pip list``
- To check list of packages available within python runtime: ``help('modules')``
- To check the version of a package within python: first import the package ``import pandas`` then check version ``pandas.__version__``
- To upgrade a package: ``pip install packagename -U``
- To force install a specific version of a package: ``pip install "pandas=0.19.3" --force-reinstall``

Packaging
---------

.. code-block:: shell

    # save a text file of all required packages and their versions
    pip freeze > requirements.txt

    # to install these packages on a server or another computer
    pip install -r requirements.txt

