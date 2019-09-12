tool - Git
==========
- Git is a what's called a Version Control System (VCS).
- Git is not the same as GitHub or GitLab. Git is a tool in itself.
  It is tool that takes complete snapshots of your work with each commit ("save")
  thereby allowing the user to roll an entire project forward and aft in time without
  having to worry about piecing the project back together.
- Git can be `downloaded <https://git-scm.com/>`_ for windows in a form of a linux emulator (GitBash), whilst
  mac and linux distributions may already have it installed, and the user can work with it
  100% offline if that is desired, however cloud providers such as GitHub and GitLab offer a collaborative online
  experience for a team of developers.

Setup
-----
- Set In your username in the terminal ``git config --global user.name "Viktor Kis"``
- Set your email address: ``git config --global user.email name@example.com``
- Set your editor: ``git config --global core.editor vim``
- Check your user setttings: ``git config --list``

Commands
--------
General

- To initialize a folder: ``git init`` or clone an existing repo ``git clone url``

    - Note that ``git clone url`` sets up your remote link, ``git init`` does not (see remote below to link up a initialized project)

- To add file(s) in queue for save: all files ``git add .`` single file ``git add filename``
- To remove a already added file from queue: ``git reset``
- To commit a change: ``git commit -m "msg with your commit"``
- To push a commit to the cloud: ``git push``
- To pull the latest data from the branch: ``git pull`` or explicitly ``git pull origin master`` (note that ``git fetch`` works similarly, however it does not merge the work with your local changes)

Status

- To check change status: ``git status``
- To check the past commit logs: ``git log --graph`` to limit the log ``git log --since=2.weeks``

Branches

- To create a new branch: ``git checkout -b branchname``
- To switch between branches: ``git checkout branchname``
- To merge a branch onto another: ``git merge``

Ignore Files

- To create a ignore files/file-types, create a .gitignore: ``touch .gitignore``
- By practice developers should only commit source files (no binaries, no .pyc files, no config files and etc.) ex; ``*.pyc``

Remote

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

- To create a branch: ``git branch branch_name``
- To work/change your current branch: ``git checkout branch_name``
