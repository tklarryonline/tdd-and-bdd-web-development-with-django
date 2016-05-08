# Serving the homepage, TDD style

Time to create some content! Yay!

Every website needs a homepage. Be it a blog, a fashion website, or even Twitter. In our current stage, the homepage should be pretty much a static landing page. Let's define our homepage as:

* Having a navigation bar with Bookshelves logo on the left hand side.
* Having a welcome text i.e. *"Hey there! Welcome to Bookshelves community!"*
* Having a footer with copyright information.

You can also go on to define other static pages like *About us* page. But here is one common thing: serving each page requires simple and basic HTML templates. We can extract these templates into core skeletons on which the whole project can build up. Let's create a `core` app for this sole purpose: serving those pages.

> "The art of creating and maintaining a good Django app is that it should follow the truncated Unix philosophy according to Douglas McIlroy: 'Write program that do one thing and do it well.'"  
> — James Bennett

## Your first Django app

### Kickstart a new app

Firstly, we need to create a new Git branch for our `core` app.

```sh
$ git checkout -b feature/serving-homepage
```

> ### Branching tip
> As a good practice,  we should develop new feature, fix or patch for the project in a branch. Branches should be namespaced i.e. `feature/` for new features, `refactor/` for patches or `bug/` for bug fixings. This is to make sure that your new code won't interfere with the current code-base in the main branch. When a branch is finished, you can create a Pull Request and then merge it into the main branch (which is most of the time `master`).
>
> You can read more about the [Feature Branch Workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow) in Atlassian's blog. 

In this new branch, you can now create your app. Run the following command in your terminal from where the `manage.py` file is (should be `bookshelves` folder).

```sh
$ python manage.py startapp core
```

This will create a new folder named `core` in `bookshelves` directory. Our directories and files now should look like this:

```
django_bookshelves
├── bookshelves/
│   ├── bookshelves_config/
│   │   ├── __init__.py
│   │   ├── settings/
│   │   ├── urls.py
│   │   └── wsgi.py
│   ├── core/
│   │   ├── __init__.py
│   │   ├── admin.py
│   │   ├── apps.py
│   │   ├── migrations/
│   │   ├── models.py
│   │   ├── tests.py
│   │   └── views.py
│   ├── manage.py
│   └── migrations/
├── readme.md
└── requirements/
    ├── base.txt
    ├── dev.txt
    └── production.txt
```

### Tell Django about this app

Now we should tell Django to use our new `core` app by editing the `INSTALLED_APPS` settings. Since we will add more apps in the future and install more third-party apps for new functions, we will restructure the `INSTALLED_APPS` settings as followed.

```py
# bookshelves_config/settings/base.py
# Old INSTALLED_APPS
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]

# Now, to our brand new INSTALLED_APPS
DJANGO_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]

THIRD_PARTY_APPS = []

LOCAL_APPS = [
    'core',
]

INSTALLED_APPS = DJANGO_APPS + THIRD_PARTY_APPS + LOCAL_APPS
```

In the future, we can add more apps into each category and don't have to worry about their mixing together.

Now you can commit the new changes to reflect the creation of our new app.

