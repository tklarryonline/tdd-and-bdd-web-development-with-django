# Serving the homepage, TDD style

Time to create some content! Yay!

Every website needs a homepage. Be it a blog, a fashion website, or even Twitter. In our current stage, the homepage should be pretty much a static landing page. Let's define our homepage as:

* Having a welcome text i.e. *"Hey there! Welcome to Bookshelves community!"*
* Having a navigation bar with Bookshelves logo on the left hand side.
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

### Extend the app structure

The generated app structure doesn't suite for an extensible app. Thus, we prefer changing it to:

```
core/
├── __init__.py
├── apps.py
├── migrations/
│   ├── __init__.py
├── tests/
│   ├── __init__.py
└── views/
    ├── __init__.py
```

Moreover, since we have no need for models and admin functions in this `core` app at the moment, we have got rid of `models.py` and `admin.py` files.

> ### Tip
> In Python, a `*.py` file is a module, a folder containing `__init__.py` file inside it is a package. You can read more about this in this topic [Module vs Package](http://programmers.stackexchange.com/questions/111871/module-vs-package).

Now you can commit the new changes to reflect the creation of our new app.

## Test-driven way to serve the homepage

Before doing anything, let's learn a bit about Test-driven Development by reading this [Newbie's Guide](http://code.tutsplus.com/tutorials/the-newbies-guide-to-test-driven-development--net-13835) from Tutsplus.

### Write your first test

Since we're going to serve the homepage, which is conventionally defined in a class `IndexView`, we will write our expectations for it first in the form of `TestCase`.

Create a new Python package named `test_views` in this directory `django_bookshelves/bookshelves/core/tests/`. Then, add a new module named `test_index_view` in it.

Our first test case is to check if the homepage is live.

```py
# core/tests/test_views/test_index_view.py
from django.core.urlresolvers import reverse
from django.test.testcases import TestCase


class IndexViewTestCase(TestCase):
    def test_homepage_is_alive(self):
        response = self.client.get(reverse('core:index'))

        self.assertEqual(response.status_code, 200)
```

Run this test at the whereabouts of our `manage.py` by this command `python manage.py test`. You will see this

```sh
$ python manage.py test
Creating test database for alias 'default'...
E
======================================================================
ERROR: test_homepage_is_alive (core.tests.test_views.test_index_view.IndexViewTestCase)
----------------------------------------------------------------------
Traceback (most recent call last):
...
django.core.urlresolvers.NoReverseMatch: 'core' is not a registered namespace

----------------------------------------------------------------------
Ran 1 test in 0.003s

FAILED (errors=1)
Destroying test database for alias 'default'...
```

We're expecting this. Because we haven't defined any rules for the URL reverse to work and resolve the URL for our homepage. Let's do it now by creating a new file named `urls.py` in our `core` app folder and populate it with

```py
# core/urls.py
from django.conf.urls import url

from .views.index_view import IndexView


urlpatterns = [
    url(r'^$', IndexView.as_view(), name='index'),
]
```

Then, include the `core` URLs settings in the main URLs.

```py
# bookshelves_config/urls.py
from django.conf.urls import url, include
from django.contrib import admin

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'', include('core.urls', namespace='core'))
]
```

Rerun the test. This time, the error changes to

```sh
python manage.py test
Creating test database for alias 'default'...
E
======================================================================
ERROR: test_homepage_is_alive (core.tests.test_views.test_index_view.IndexViewTestCase)
----------------------------------------------------------------------
Traceback (most recent call last):
...
ImportError: No module named 'core.views.index_view'
```

Right! Because we haven't created `index_view` module! Let's make it right away in the `views` folder

```py
# core/views/index_view.py
from django.http.response import HttpResponse
from django.views.generic import View


class IndexView(View):
    def get(self, request, *args, **kwargs):
        return HttpResponse('Hey there! Welcome to Bookshelves community!')
```