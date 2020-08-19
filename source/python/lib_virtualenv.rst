lib - virtualenv
================
Developers use virtual environments for 2 main reasons:

- Isolating different projects on 1 machine and managing their dependencies
- Supporting various versions of python and/or other 3rd party libraries for a single project

There are a few options out there for virtual environments:
- venv: comes with python and it is very basic
- virtualenv: pip installed library, i feel like i had better success using pyinstaller with virtualenv vs venv
- pipenv: pip installed library, gives better dependency descriptions and git-branch description
- conda: shipped with anadonda, very similar to virtualenv

1) pip install virtualenv (version 16.1.0 for win10 build compatibility) in your global site-packages

.. code-block:: shell

    pip install virtualenv==16.1.0

2) Create your git repo online and clone it down (see :doc:`tool_git`)

2) Browse into your git repo and create your virtualenv from terminal:

.. code-block:: shell

    python -m virtualenv venv_projectname --no-site-packages

Note as a convention, I recommend placing the venv inside your git repo folder so that everything is together.
This setup integrates really nicely with Editors like PyCharm where the project recognizes that you created a
virtualenv inside your git repo folder.

This will create you a folder with bear minimum python packages (this is nice if you would like to
pyinstaller package up your work that ONLY use the packages required for your project). The folder created
has a bunch of folders.

 - *Lib/Site-Package*: has your packages installed, if you want to manually add a module - this is where you would place it
 - *Scripts*: has your activate/deactive, and all your run files like python.exe, pip etc.

3) Activate your virtualenv (with Gitbash, assuming you are one dir higher than your virtualenv)

.. code-block:: shell

    source venv_projectname/Scripts/activate

4) Deactive

.. code-block:: shell

    deactivate

5) To ensure we are not pushing unnecessary items to our git repo, we can create a ``.gitignore`` file.
Create a ``.gitignore`` (from terminal: ``touch .gitignore`` and you will most likely want the following in it:

::

    Include
    Lib
    Scripts
    tcl

    __pycache__
    *.pyc

    build
    dist
    venv_projectname
