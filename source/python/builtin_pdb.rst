builtin - pdb (python debugger)
===============================
PDB is python native debugger, it comes with every python installation therefore it is reliable
across all platforms (wheather you are on a coworkers PC or ssh'ed into a remote server).

- execute your code via pdb: ``python -m pdb file.py``
- insert a breakpoint into your script via ``import pdb`` then ``pdb.set_trace()`` above the line you
  would like to break at
- jump to next line ``n``
- jump to next breakpoint ``x``

