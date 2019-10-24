lib - PyQt5 (GUI)
=================

Installation
------------
.. code-block:: shell

    pip install pyqt5 pyqt5-tools

Designer (GUI builder)
----------------------
PyQt comes with an awesome drag and drop GUI builder ``designer.exe``.
The tool (and all other pyqt .exe) will be placed in:

- lib/site-package/pyqt5_tools/Qt/bin
- or if you are using virtualenv, it will simply be under Scripts

1) Construct your GUI by drag/drop method and save it as a ``.ui``
2) Convert your ``.ui`` to python code with ``pyuic5.exe``

.. code-block:: shell

    # -x to make it executable (creates __name__ == "__main__")
    # -o to specify output filename
    pyuic5.exe -x -o outputfilename.py designerfilename.ui


Designer to Python code setup
-----------------------------
An efficient way to use Qt Designer is export out the widget code then without alterations import it
into the logic modules.

.. code-block:: python

    from form import Ui_MainWindow
    from PyQt5 import QtWidgets, QtCore, QtGui

    class Ui(QtWidgets.QMainWindow, Ui_MainWindow):
        def __init__(self, *args, **kwargs):
            QtWidgets.QMainWindow.__init__(self, *args, **kwargs)
            self.setupUi(self)

            # to enable event handling
            self.installEventFilter(self)

        # overwrite the Qt event method
        def eventFilter(self, source, event):
            if (event.type() == QtCore.QEvent.KeyPress and
                    event.matches(QtGui.QKeySequence.Copy)):
                # now pipe the event to any method to logic handling
                self.customcopy()
            # exp TBD
            return super(Ui, self).eventFilter(source, event)

        def customcopy(self):
            pass

    if __name__ == "__main__":
        # create an instance of Qt (pass in sys.argv allows args to be passed it from terminal)
        app = QtWidgets.QApplication(sys.argv)

        gui = Ui()
        gui.show()
        # app.exce_() runs the mainloop, and returns 0 for no error, 1 for error
        sys.exit(app.exec_())



Tables
------

.. code-block:: python

    # NOTE: this is another method to the example shown above under "Designer to Python code setup"

    # set cell value
    def mycellsetter(self, value):
        # input value must be a string
        row = 0
        col = 0
        self.table.setItem(row,col,QtWidgets.QTableWidgetItem(str(value)))

    # to get cell value
    def mycellgetter(self):
        row = 0
        col = 0
        # return values will always be strings
        return self.table.item(row, col).text()

    # to iterate through a tableWidget
    def tableiter(self):
        maxcol = self.table.model().columnCount()
        maxrow = self.table.model().rowCount()
        for c in range(maxcol):
            for r in range(maxrow):
                # note that empty cells show up as None type
                if self.table.item(r,c) != None:
                    # to get the actual value stored we have to call .text() on the current cell
                    self.table.item(r,c).text()

Path File Browser
-----------------

.. code-block:: python

    # NOTE: this is another method to the example shown above under "Designer to Python code setup"

    def getpath(self):
        path = QtWidgets.QFileDialog.getExistingDirectory(self, 'Select Directory')
        return path


MessageBox Popup
----------------

.. code-block:: python

    # NOTE: this is another method to the example shown above under "Designer to Python code setup"

    # the following is useful as error handling popup
    try:
        # some code
    except Exception as e:
        msgbox = QtWidgets.QErrorMessage(self)
        msgbox.showMessage(str(e))


tabWidget Indexing
------------------

.. code-block:: python

    def tabpiper(self):
        if self.yourtabwidgetname.currentIndex() == 0:
            print('you are on the first tab')
        elif self.yourtabwidgetname.currentIndex() == 1:
            print('you are on the second tab')



PyInstaller Packing TroubleShooting
-----------------------------------
Dealing with "ImportError: unable to find QtCore.dll on PATH"
Run on pyinstaller 3.5 and PyQt5 5.12.3
Create spec file via (pyi-makespec filename.py)
`PyInstaller Link <https://pyinstaller.readthedocs.io/en/stable/man/pyi-makespec.html>`_
Add to gui.spec datas=[('fullpath/site-packages/PyQt5/Qt/bin/*','PyQt5/Qt/bin')]
then run pyinstaller gui.spec --onefile

GUI Lockup - Multithreading
---------------------------
TBD. QWidgets.QApplication.processEvents() # execute a second window without locking up the first. Or multi-tread