package - virtualenv
====================

1) pip install virtualenv (version 16.1.0 for win10 build compatibility)

.. code-block:: shell

    pip install virtualenv==16.1.0

2) This will create you a folder with bear minimum python packages (this is nice if you would like to
pyinstaller package up your work that ONLY use the packages required for your project). The folder created
has a bunch of folders.
 - *Lib/Site-Package*: has your packages installed, if you want to manually add a module - this is where you would place it
 - *Scripts*: has your activate/deactive, and all your run files like python.exe, pip etc.

3) Activate your virtualenv (with Gitbash, assuming you are one dir higher than your virtualenv)

.. code-block:: shell

    source foldername/Scripts/activate

4) Deactive

.. code-block:: shell

    deactivate

5) As a convention, place a 00_project folder within the virtualenv, and this is where you would git initialize