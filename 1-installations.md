# Installations

Before doing any things, you should make sure that you are having a central place  to store your working project folders. In this tutorial, we will assume that place is at `~/django_tutorial/`. We will continue every of our work in this directory.

## Install and get started with Git

Almost every software development project depends in one way or another on a version control system. In our professional practice, we use Git for this purpose. Since its use is common and important, we would recommend to install Git at this early stage.

### Install Git

You could easily Google about installing Git into your platform, or simply follow the instruction at [Installing Git section of *Pro Git*](http://www.git-scm.com/book/en/Getting-Started-Installing-Git).

After having Git installed, you can start by initialize your project as a repository (We will refer it as **repo**). First, create a `django_bookshelves` directory as our project folder.

```sh
# Remember to change current working directory to where you want to store the project folder
cd ~/django_tutorial

# Get into the project folder
mkdir django_bookshelves
```

Make `django_bookshelves` become the Git repo:

```sh
cd django_bookshelves
git init
```

If you haven't used Git before, you will also want to personalize it. This is a good [Git setup and configure guide from GitHub](https://help.github.com/articles/set-up-git).

### Create a GitHub account

GitHub provides unlimited public Git repositories for its free users. It's also the place for developers to share their knowledge or projects.

Go to [GitHub.com](https://github.com) and sign up for a new, free user account.

### Store your repo on GitHub

Follow the first part of [this GitHub repo guide](https://help.github.com/articles/create-a-repo) to create a new online repo. In case you did not use github-ssh-key yet, follow [this guide](https://help.github.com/articles/generating-ssh-keys) to create one.

After that, add the GitHub remote to your local repo

```sh
git remote add origin git@github.com:<your_username>/<your_repo_name_on_GitHub>.git
```

From now on, whenever you want to store your commits on GitHub, you could just

```sh
git push origin master
```