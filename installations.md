# Installations

Before doing any things, you should make sure that you are having a central place  to store your working project folders. In this tutorial, we will assume that place is at `~/django_tutorial/`. We will continue every of our work in this directory.

## Install and get started with Git

Almost every software development project depends in one way or another on a version control system. In our professional practice, we use Git for this purpose. Since its use is common and important, we would recommend to install Git at this early stage.

### Install Git

You could easily Google about installing Git into your platform, or simply follow the instruction at [Installing Git section of *Pro Git*](http://www.git-scm.com/book/en/Getting-Started-Installing-Git).

After having Git installed, you can start by initialize your project as a repository (We will refer it as **repo**). First, create a `django_bookshelves` directory as our project folder.

```sh
# Remember to change current working directory to where you want to store the project folder
$ cd ~/django_tutorial

# Get into the project folder
$ mkdir django_bookshelves
```

Make `django_bookshelves` become the Git repo:

```sh
$ cd django_bookshelves
$ git init
```

If you haven't used Git before, you will also want to personalize it. This is a good [Git setup and configure guide from GitHub](https://help.github.com/articles/set-up-git).

### Create a GitHub account

GitHub provides unlimited public Git repositories for its free users. It's also the place for developers to share their knowledge or projects.

Go to [GitHub.com](https://github.com) and sign up for a new, free user account.

### Store your repo on GitHub

Follow the first part of [this GitHub repo guide](https://help.github.com/articles/create-a-repo) to create a new online repo. In case you did not use github-ssh-key yet, follow [this guide](https://help.github.com/articles/generating-ssh-keys) to create one.

After that, add the GitHub remote to your local repo

```sh
$ git remote add origin git@github.com:<your_username>/<your_repo_name_on_GitHub>.git
```

From now on, whenever you want to store your commits on GitHub, you could just

```sh
$ git push origin master
```

## Manage Python versions with pyenv

### Install pyenv

Pyenv is a simple Python version management. It allows us to install different Python versions on one system and switch version easily for different projects.

First, we will clone pyenv into `~/.pyenv`

```sh
$ git clone https://github.com/yyuu/pyenv.git ~/.pyenv
```

Next, we will modify shell configuration to add pyenv path to `$PATH` to access `pyenv` command-line utility. Add these lines at the end of `~/.bashrc`, or `~/.zshrc` if you use Zsh.

```sh
export PYENV_ROOT="$HOME/.pyenv"
export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init -)"
```

Restart the shell and check if pyenv is setup correctly

```
$ pyenv --version
```

If pyenv works, it will return something like `pyenv 20151222-8-g5c5205e`

You can [read more about the commands in pyenv](https://github.com/yyuu/pyenv/blob/master/COMMANDS.md).

### Install Python

To make it consistent and updated, in this tutorial we will use `Python-3.5.1`.

First, install Python via pyenv

```sh
$ pyenv install 3.5.1
```

Then we will set this version of Python as default Python version for system

```
$ pyenv global 3.5.1
```

You can check if Python 3.5.1 is installed and used as default Python in a new shell session

```sh
$ pyenv versions
  system
* 3.5.1 (set by /path/to/.pyenv/version)
```

## Install virtualenv

**`Virtualenv`** is a tool to create isolated Python environments. Thus you can have different environments for each projects.

We have 2 options for managing `virtualenv` with pyenv:

* pyenv-virtualenv
* pyenv-virtualenvwrapper

You can choose any of those 2 options for your project. We used settings which isolated 2 options so they won't (hopefully) conflict with each other on same system.

We will start by using `pip` to install `virtualenv` and `virtualenvwrapper` package on our global Python version. `pip` is a tool to install Python packages and it is installed by default when we install a Python version through pyenv.

```
$ pip install virtualenv virtualenvwrapper
```

### Option 1: Install pyenv-virtualenv

Clone `pyenv-virtualenv` into pyenv's plugins directory

```
$ git clone https://github.com/yyuu/pyenv-virtualenv.git ~/.pyenv/plugins/pyenv-virtualenv
```

**Optionally**, you can enable auto-activation of virtualenvs. Add this line after pyenv configuration in `~/.bashrc` (or `~/.zshrc`) and restart your shell.

```sh
eval "$(pyenv virtualenv-init -)"
```

#### Usage:

Create a new virtualenv (this will create virtualenv directory in `/path/to/.pyenv/versions)

```
$ pyenv virtualenv 3.5.1 django_venv  # we can drop Python version argument here if we want to create virtualenv with current active Python
```

List virtualenvs

```sh
$ pyenv virtualenvs
* django_venv (created from /path/to/.pyenv/versions/3.5.1)
```

Activate/Deactivate virtualenv

```
$ pyenv activate django_venv
$ pyenv deactivate
```

Delete virtualenv

```
$ pyenv uninstall django_venv
```

### Option 2: Installing pyenv-virtualenvwrapper

Clone pyenv-virtualenvwrapper into pyenv's plugins directory

```
$ git clone https://github.com/yyuu/pyenv-virtualenvwrapper.git ~/.pyenv/plugins/pyenv-virtualenvwrapper
```

Add these lines after pyenv configuration in `~/.bashrc` (or `~/.zshrc`) and restart shell.

```
export WORKON_HOME=$HOME/.virtualenvs
pyenv virtualenvwrapper
```

#### Usage:

Create a new virtualenv (this will create virtualenv directory in `~/.virtualenvs`)

```
$ pyenv shell 3.5.1  # switch to Python version that you want to create virtualenv
$ mkvirtualenv django_venv
```

List virtualenvs

```
$ lsvirtualenv -b
django_venv
```

Activate/Deactivate virtualenv

```
$ workon django_venv
$ deactivate
```

Delete virtualenv

```
$ rmvirtualenv django_venv
```