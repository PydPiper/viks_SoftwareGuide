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
- Set up your username in the terminal ``git config --global user.name "Viktor Kis"``
- Set your email address: ``git config --global user.email name@example.com``
- Set your editor: ``git config --global core.editor vim``
- Check your user setttings: ``git config --list``

Step-by-Step GitHub Repo
------------------------

1) Create a free github account at: github.com

2) On the top right click your ``user icon`` and a drop-down should appear > Click ``Your repositories``

3) On your ``Repositories`` tab towards the right, there should a green ``New`` icon, click it.

4) Give your new repo a name and hit ``Create repository``

5) Go back to the ``Repositories`` tab and select your new repo. On your repo page, towards the right there
   there should be a green ``Clone or download`` button, click it and copy the http link.

6) On your local machine, open up a terminal and type ``git clone http-for-your-repo``

7) You now have a successful link up to your online repo from your local. Let's step through a basic upload

    7.1) Add a new file to your repo folder (inside your project folder that was cloned down)

    7.2) Add it to queue: ``git add .``

    7.3) Add a commit message: ``git commit -m "initial demo upload"``

    7.4) Push it to the github: ``git push``


Commands
--------

General
^^^^^^^

- To initialize a folder: ``git init`` or clone an existing repo ``git clone url``

    - Note that ``git clone url`` sets up your remote link, ``git init`` does not (see remote below to link up a initialized project)

- To add file(s) in queue for save: all files ``git add .`` single file ``git add filename``
- To remove an already added file from queue: ``git reset``
- To commit a change: ``git commit -m "msg with your commit"``
- To push a commit to the cloud: ``git push``
- To pull the latest data from the branch: ``git pull`` or explicitly ``git pull origin master`` (note that ``git fetch`` works similarly, however it does not merge the work with your local changes)
- To completely overwrite local files with server files: ``git reset --hard origin/master``

Status
^^^^^^

- To check change status: ``git status``
- To check the past commit logs: ``git log --graph`` to limit the log ``git log --since=2.weeks``

Branches
^^^^^^^^

- To create a new branch: ``git checkout -b branchname``
- To switch between branches: ``git checkout branchname``
- To merge a branch onto another: ``git merge``

Ignore Files
^^^^^^^^^^^^

- To create an ignore files/file-types, create a .gitignore: ``touch .gitignore``
- By practice developers should only commit source files (no binaries, no .pyc files, no config files and etc.) ex; ``*.pyc``

Remote
^^^^^^

- To add a remote link ``git remote add user_defined_remote_name url``

.. code-block:: shell

    git clone github.com/project.git
    git remote -v
    >>> origin https://github.com/project.git (fetch)
    >>> origin https://github.com/project.git (push)
    # to add a link to someone's fork of the project for example, we would:
    git remote add fork_vik https://github.com/vik/project.git
    git remote -v
    >>> origin https://github.com/project.git (fetch)
    >>> origin https://github.com/project.git (push)
    >>> fork_vik https://github.com/vik/project.git (fetch)
    >>> fork_vik https://github.com/vik/project.git (push)
    # now we have to option to pull/fetch from a fork onto our local project
    git pull fork_vik master

Branch
^^^^^^

- To create a branch: ``git branch branch_name``
- To work/change your current branch: ``git checkout branch_name``

Common Issues
-------------
- "failed to push some refs to repo, tip of your current branch is behind"

    - Cause: there were changes to the remote repo that you dont have (this could be file or history log change)
    - Fix: run a ``git pull`` and resolve the conflicts

