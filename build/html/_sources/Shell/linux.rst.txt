Linux Quick Start Guide
=======================

General
-------

- To change directories: ``cd folder1/folder2``
- To back out a directory: ``cd ..`` then back out 2 and so on: ``cd ../..``
- To back out to home directory: ``cd ~``
- To create a directory: ``mkdir folder1``
- To create a file: ``touch file.txt``
- To concatenate 2 files: ``cat file1.txt file2.txt > file2.txt``

    - Pipe to a file: ``>``
    - To append to a file: ``>>``

- To to print file content to terminal: ``cat file1.txt``
- To execute a file: ``./file1.txt``
- To remove a file: ``rm file1.txt``
- To remove all files under a folder: ``rm -rf folder1``
- To remove a folder: ``rmdir folder1``
- To rename a file/folder: ``mv file1.txt file2.txt``
- To get current working directory: ``pwd``
- To get list of files/folders in your current working directory: ``dir -la`` or ``ls -la`` (-l for long desc, -a for hidden)
- To search for text in files/folders: ``grep -r "text" *`` (the "*" is a wild card)

File Permissions
----------------

- To get list of files/folders with permission levels in your current working directory: ``dir -la``
- To change file/folder permissions: ``chmod -R u=rwx folder``

    - ``-R`` is recursively change all files/folder under the given folder
    - ``u`` is for "user", ``g`` for "group", ``o`` for "other" and ``a`` for "all"
    - ``r`` for "read", ``w`` for "write", ``x`` for "execute"

- To change group of a file/folder: ``chgrp new_groupname file.txt``
- To check which groups you belong to: ``groups``


Scripting
---------

- To print datetime: ``date +%m%d%y`` (note lower case is short form, upper case is long form)
- To declare a variable in a script: ``variable1 = "this"`` and to call it ``$variable1``



