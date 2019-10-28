tool - PyCharm (IDE)
====================

General
-------

- To auto PEP8 (code cleanup/format): ``ctrl + alt + L``
- To enable Crtl+Mouse Wheel text zoom: File > Settings > Editor > General > Mouse > Change Font size...

New Project Setup
-----------------

1) If project `folder already exists: File > Open > Navigate to folder
2) Configure your python environment:
   File > Setting > Project > Project Interpreter > Gear on top right > Add >>
   Virtualenv Environment > 2 options here...

    2.1) Create a virtualenv (highly recommended, for pip version control)
         Either link to an existing virtualenv python.exe (this will be under ProjectNameVenv/Scripts/)
         or create a virtualenv directly within PyCharm (note that you have to open to inherit all global packages,
         make sure this is unchecked before creating the virtualenv)
    2.2) Link to global python interpreter (access to all global pip)

Terminal
--------

- To configure terminal start directory:

File > Settings > Tools > Terminal > Start directory

- To configure a custom terminal emulator (like gitbash) inside pycharm terminal tab:

File > Settings > Tools > Terminal > Shell path: ``"C:\Program Files\Git\bin\sh.exe" --login``


