lib - Sphinx (code documentation)
=====================================

1) Create a virtualenv, see :doc:`lib_virtualenv`
2) Active virtualenv and add sphinx ``pip install sphinx``
3) Change dir to your 00_project folder
4) Run ``sphinx-quickstart`` from the terminal (you will want to separate source and build for a cleaner structure)
5) To install a custom sphinx theme: ``pip install sphinx_rtd_theme``
6) Edit theme on your ``source/conf.py`` file to be ``sphinx_rtd_theme``
7) Create your docs

    7.1) Make sure your files have headers (line 1: header text, line2: =======)

    7.2) Make sure you add each filename to your index.rst otherwise it is not going to show up as an index

8) To create your html files ``./make.bat html``
9) To clean up your builds before git push ``./make.bat clean``
10) Push your docs to your git (make sure you do a ``pip freeze > requirements.txt`` because readthedocs might fail for
    using the incorrect version)
11) Setup your commit hock with readthedocs (login to readthedocs, add your github project, then build).
    After your build is complete, each git push will auto-trigger a webhock to readthedocs


Text Manipulations
------------------
Ref Sphinx rst docs:
- `Link1 <http://www.sphinx-doc.org/en/master/usage/restructuredtext/index.html>`_
- `Link2 <https://www.sphinx-doc.org/en/master/usage/restructuredtext/directives.html?highlight=code-block#showing-code-examples>`_

- To italicize text: ``*text*``
- To bold text: ``**text**``
- Subscript/Superscript: ``:sub:`yourtext``` or ``:sup:`yourtext```
- To in-text code highlight: ````text````
- Important messages: ``.. note::`` ``.. warning::`` ``.. deprecated::`` ``.. seealso::``
- Internal Links: ``:doc:`filename```
- External Links: ```linktext <https://google.com>`_``
- Section Links within the same doc

    1) put a ``.. _ref1:`` above the header you want to ref (make sure there is an empty line between the header and _ref1)
    2) call the link by: ``:ref:`ref1```
- Citations: to define it ``.. [refname1] Title`` then to use it in text: ``[refname1]_``
- Today's date in text ``|today|``
- Bullets, there are a few not so intuitive conventions about bullets, see below:

.. code-block:: text

    - this is a bullet

    - this is another bullet

    - this is a bullet
    on multi lines

    1) this is a numbered bullet

        1.1) this is a nested bullet
        with multi lines

        1.2) here is another nested bullet

Header Types
------------

- Title: ###
- Section: ===
- SubSection: ---
- 2SubSection: ^^^
- 3SubSection: """


Sphinx toctree
--------------
1) The ``.. toctree::`` must be present in your top level index.rst file. The links will show up on both your page and
   on the left quickbar
2) The sub-folder trees also need to have a index.rst with ``.. toctree::`` to properly get nested tree reference links,
   see example below:

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

    .. autoclass::
        :members:

    .. autofunction:: folder.file.functionname

Figures
-------

.. code-block:: text

    .. figure:: pic.png
        :scale: 50%
        :alt: Alternative text if image does not load, spoken by application for visually impaired
        :align: center

        This is caption text