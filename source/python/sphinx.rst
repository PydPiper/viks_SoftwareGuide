Sphinx Quick Start Guide
========================

1) Create a virtualenv, see :doc:`virtualenv`
2) Active virtualenv and add sphinx ``pip install sphinx``
3) Change dir to your 00_project folder
4) Run sphinx-quickstart (you will want to separate source and build for a cleaner structure)
5) To install a custom sphinx theme: ``pip install sphinx_rtd_theme``
6) Edit theme on your source/conf.py file to be "sphinx_rtd_theme"
7) Create your docs
8) To create your html files ``./make.bat html``
9) To clean up your builds before git push ``./make.bat clean``
10) Push your docs to your git
11) Setup your commit hock with readthedocs


Text Manipulations
------------------
Ref Sphinx rst docs:
- `Link1 <http://www.sphinx-doc.org/en/master/usage/restructuredtext/index.html>`_
- `Link2 <https://www.sphinx-doc.org/en/master/usage/restructuredtext/directives.html?highlight=code-block#showing-code-examples>`_

- To itilosize text: ``*text*``
- To bold text: ``**text**``
- Subscript/Superscript: ``:sub:`yourtext``` or ``:sup:`yourtext```
- To in-text code highlight: ````text````
- Important messages: ``.. note::`` ``.. warning::`` ``.. deprecated::`` ``.. seealso::``
- Internal Links: ``:doc:`filename```
- External Links: ```linktext <https://google.com>`_``
- Today's date in text ``|today|``
- Section underline: Section:``====``, Sub-Section ``----``, Sub-Sub-Section ``^^^^``

Sphinx toctree
--------------
1) The ``.. toctree::`` must be present in your top level index.rst file. The links will show up on both your page and on the left quickbar
2) The sub-folder trees also need to have a index.rst with ``.. toctree::`` to properly get nested tree reference links, see example below:

.. code-block:: text

    on the top-level index.rst
    .. toctree::

        python/index
        VBA/index
        Shell/index

.. code-block:: text

    in the subdirectory /python/ we have a index.rst, it needs to have a header and a sub-toctree

    Python Guide
    ============

    .. toctree::

        virtualenv
        sphinx

Code-Blocks
-----------

.. note:: **".. code-block::" has to have an empty line above and below it, AND empty line after your code.
            The code has to be on the same indent level as ":linenos"**

.. code-block:: text

    .. code-block:: shell
        :linenos:
        :lineno-start: 10
        :emphasize-lines: 3,5

        some shell code

Code-Auto-Doc
-------------
1) Uncomment the following from your config file:

.. code-block:: text

    import os
    import sys
    sys.path.insert(0, os.path.abspath('.'))

2) In your desired .rst file, add the following (where each function/class is the member):

.. code-block:: text

    .. automodule::
        :members: foo, bar

Figures
-------

.. code-block:: text

    .. figure:: pic.png
        :scale: 50%
        :alt: Alternative text if image does not load, spoken by application for visually impaired
        :align: center

        This is caption text