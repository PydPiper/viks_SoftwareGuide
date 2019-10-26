lib - pyinstaller
=================
pyinstaller is a packaging module for python projects. It is most efficiently used
with virtualenv where only the python packages needed for the project are packaged
up (thereby creating a smaller .exe).

Installation
------------

.. code-block:: shell

    pip install pyinstaller

Usage
-----

1) create a virtualenv and activate it
2) install necessary python packages for the project
3) complete the project
4) run pyinstaller
4.1) pyinstaller does not need to be locally installed within your project for it package up your project,
however it does help to keep a stable working version locked with your project. If your global python path is
setup (you are able to type ``python`` in your terminal), then ``pyinstaller`` should also work if it is installed.
Normally pyinstaller will be in the Scripts folder
4.2) running pyinstaller: ``pyinstaller yourmodule.py --onefile``
4.3) if everything goes well this will create 3 items:

- build folder: contains compiled parts of your package and log files
- dist folder: contains your single .exe file
- .spec file: instructions for pyinstaller to create an executable


TroubleShooting
---------------

1) always check your package runs before compiling
2) build the project and see what the error log says when running the .exe
it is usually an import error or version incompatability.

- Fixing import error. Edit your spec file to include location of missing module, for example for pyqt5

::

    ...
    datas=[('c:/users/username/projectvenv/Lib/site-packages/PyQt5/Qt/bin/*','PyQt5/Qt/bin')],
    ...

- then compile your project via .spec file ``pyinstaller project.spec``. If you dont have a spec file,
create one via ``pyi-makespec project --onefile``
