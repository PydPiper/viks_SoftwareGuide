tool - vim (text editor)
========================
Why use vim?
- vim is installed on every linux machine, therefore knowing how to work it will ensure
  that you are able to work any where. New machine, coworker's machine, remote servers (where your
  favourite editor cannot be installed/used)

- How to start vim on terminal: ``vim`` or ``vim filename``
- By default vim opens in ``command mode`` **you cannot edit the file from this mode**
   to enter ``insert mode`` press ``i`` or ``insert`` key. To get back to ``commad mode`` press ``esc``
- command window - write to file and quit: ``:wq``
- command window - quit without saving: ``q!``:
- command window - increase font size: ``ctrl`` + ``=``
- command window - decrease font size: ``ctrl`` + ``-``
- command window - add syntax highlighting ``syntax on`` ``syntax off``
- command window - add line numbers ``set number`` ``set nonumber``
- command window - jump to the beginning of a file ``gg`` jump to the end of a file ``shift`` + ``g``
- command window - jump to a line number ``:100`` to jump to line 100
- command window - undo last command ``u``
- insert mode - increase indent: ``ctrl`` + ``t``
- insert mode - decrease indent: ``ctrl`` + ``d``


In addition you can launch a vim tutorial in your terminal by typing ``vimtutor`` in your terminal

searching
---------

1) enter ``command window``
2) type ``/`` followed by the character or string to search, ex: ``/hello world`` then press ``enter``
3) press ``n`` to search forward or ``N`` to search backward

