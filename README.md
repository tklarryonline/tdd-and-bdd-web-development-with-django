# TDD and BDD Web Development with Django

## Introduction

Our aim in writing this tutorial is to share all the practices and ideas to build a simple web application using Django.

We hope that we could make you love Django and Web technologies as much as we do.

## What you will learn in this tutorial

In this tutorial, we will walk you through the creation of a basic bookshelves web application to keep track and lend books. We will show you how to put (deploy) it online for your friends to use!

Your app will consist of two parts:

* A public site letting people to keep track of their books and who borrowed them.
* An admin interface to manage the users and book categories. Admins can also view the statistical reports from the usage data.

You will also learn:

* To build your application using TDD and BDD practices.
* To control the source version using Git.
* To use CSS Frameworks to make your web application look neat without any hassle.
* To deploy your app to Heroku and show it to your friends.

Okay? Let's start!

## Conventions used in this tutorial

Throughout this tutorial, we use code blocks like the following:

```py
# book.py
class Book(object):
    def __init__(self):
        self.is_borrowed = False
```

Other typical conventions that we are also using are:

* `Constant width` for commands or small pieces of code.
* *Italic* for file names.
* **Bold** to introduce new terms or important words.

Quotes are for notes, warning and tips, and tiny jokes.

> ### Notes
> Well, just some note as an example.

We can also use tables to summarize our ideas or information in a handy way.

## Following this tutorial

We recommend that you use Mac OS X or Linux to follow this tutorial. Windows makes it hard to install and start any Python projects. Thus, please consider carefully before choosing to use Windows for development.

## About and contributing

This tutorial is being actively maintained by [Luan Nguyen](https://github.com/tklarryonline). Should you find any mistakes or want to contribute your ideas, please submit an Issue or Pull Request to [this repository](https://github.com/tklarryonline/tdd-and-bdd-web-development-with-django).
