python cheatsheet
=================
This is a collector of miscellaneous commands that do not warrant a designated page for each, however
super useful when looking for its syntax

Packaging
---------
To check which python version is being run: ``sys.version_info[0] >= 3``

Files
-----
To compile .py to .pyc: ``python -m py_compile file.py``
To decompile a .pyc to .py: ``pip install decompyle6`` then ``decompyle6 file.pyc > file.py``
