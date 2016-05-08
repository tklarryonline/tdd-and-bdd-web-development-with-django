# Setup your databases

> Should have Django-environ setup and running

There is a lot of different databases to store data for our project. We will choose PostgreSQL as our backend database. You can, however, choose any other database system like SQLite, MySQL. This is just the matter of self preference.

## Install PostgreSQL

There is a [detailed installation guides](https://wiki.postgresql.org/wiki/Detailed_installation_guides) published by the guys behind PostgreSQL. You just have to follow it to install the correspondent database into your system (Mac OS X, Ubuntu, etc.).

## Make Django work with PostgreSQL

It is great that since version 1.8, Django has had native support for PostgreSQL. But first, you will have to install `psycopg2`, the most popular PostgreSQL adapter for Python. Then, config Django to connect to the database via that adapter.

### Install `psycopg2`

Install `psycopg2` into your virtual environment (`django-bookshelves`, remember?)

```sh
$ pip install psycopg2
Collecting psycopg2
...
```

Log the installed packages into your `requirements.txt` file and commit your changes.

```sh
# at ~/django_tutorial/django_bookshelves/
$ pip freeze > requirements.txt

$ git add requirements.txt

$ git commit -m "Install psycopg2 to make PostgreSQL work with Python"  # Change the commit message if you want
```

### Configure PostgreSQL

It's generally more secured to not using your master PostgreSQL account to do all the works for any of your applications. Thus, we will create a database for this project and a user with permissions for this database only.

First, create a database named `django_bookshelves` for our purpose

```sh
$ createdb django_bookshelves
```

Now create your database user with the following:

```sh
$ createuser -P
```

You will meet with a prompt to create new user. Let's also name it `django_bookshelves`. After the name and password, you will need to ensure that this new user only has access to what you gave it access to and nothing else.

Now, connect to PostgreSQL command-line interface using `psql` command. You will see this kind of interface in your terminal

```
psql (9.4.1, server 9.3.1)
Type "help" for help.

your_master_account_name=# <== Where you can write SQL commands.
```

Grant this `django_bookshelves` user all privileges on the newly created database using the `grant` SQL command.

```
# grant all privileges on database django_bookshelves to django_bookshelves;
```

Now, any database clients can connect to your database by the account above using this simple URL:

```
postgres://<user>:<password>@<host><:port>/<database_name>
```

In our case, it should be like below.

```
postgres://django_bookshelves:<your_password>@localhost/django_bookshelves
```

> ### Notes
> Unless you purposely changed the serving port of your PostgreSQL setup, you don't need to include the port number in the database connection URL.

Take note the URL. We will use it to setup Django database connection later

### Wire up Django's part

Change the `DATABASES` setting to connect to PostgreSQL by default

```py
# bookshelves/bookshelves_config/settings/base.py
DATABASES = {
    'default': env.db('DATABASE_URL')
}
```

Then set the `DATABASE_URL` environment variable in your `.env` file to what we had in the last section.

```sh
# .env
DATABASE_URL=postgres://django_bookshelves:<your_password>@localhost/django_bookshelves
```
