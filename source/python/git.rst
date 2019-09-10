tool - Git
==========
- Git is what's called a Version Control System (VCS).
- Git is not the same as GitHub or GitLab. Git is a tool in itself.
  It is a tool that takes a complete snapshots of your work with each commit ("save")
  thereby allowing the user to roll an entire project forward and aft in time without
  having to worry about piecing the project back together.
- Git can be `downloaded <https://git-scm.com/>`_ for windows in a form of a linux emulator (GitBash), whilst
  mac and linux distributions may already have it installed. The user can work with it
  100% offline if that is desired, however cloud providers such as GitHub and GitLab offer a collaborative online
  experience for a team of developers.

Setup
-----
- Set in your username in the terminal ``git config --global user.name "Viktor Kis"``
- Set your email address: ``git config --global user.email name@example.com``
- Set your editor: ``git config --global core.editor vim``
- Check your user setttings: ``git config --list``

Commands
--------
General

- To initialize a folder: ``git init`` or clone an existing repo ``git clone url``
- To add file(s) in queue for save: all files ``git add .`` single file ``git add filename``
- To remove an already added file from queue: ``git reset``
- To commit a change: ``git commit -m "msg with your commit"``
- To push a commit to the cloud: ``git push``

Status

- To check change status: ``git status``
- To check the past commit logs: ``git log --graph`` to limit the log ``git log --since=2.weeks``

Branches

- To create a new branch: ``git checkout -b branchname``
- To switch between branches: ``git checkout branchname``
- To merge a branch onto another: ``git merge``

Ignore Files

- To create an ignore files/file-types, create a .gitignore: ``touch .gitignore``
- By practice developers should only commit source files (no binaries, no .pyc files, no config files and etc.) ex; ``*.pyc``
