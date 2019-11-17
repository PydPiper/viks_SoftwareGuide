lib - Django (web framework)
============================
Django is one of many web frameworks out there available for use by python developers. Django is by far
one of the most popular frameworks out there because it is easy to use yet it has a lot of depth to it
(very similar to the python language itself).

Quick Setup Guide
-----------------

1.) Create a git repo and clone it down (see :doc:`tool_git`)

2.) Create a venv within the repo and activate the venv (see :doc:`lib_virtualenv`)

3.) Install required packages: ``pip install django``

4.) Setup Django Project Structure: ``django-admin startproject projectname``

5.) Restructure Folder: Django creates a sub-folder within a "projectname" this is a bit redundant, here is an easier setup:

::

    git_repo
       |
       |-projectname
       |  |
       |  |-__init__.py
       |  |-settings.py
       |  |-urls.py
       |  |-wsgi.py
       |-manage.py

6.) Check that the initial setup was successful, locally launch the site: ``python manage.py runserver``

7.) Create a new app ``python manage.py startapp appname``

    7.1) ``admin.py`` is your django admin settings

    7.2) ``apps.py`` is your app settings

    7.3) ``models.py`` is your database tables

    7.4) ``test.py`` test cases

    7.5) ``views.py`` settings for what will be displayed as HTML

8.) After a new app is created it must be added to the ``settings.py`` > ``INSTALLED_APPS`` list
It is the directory name of the app that is to be added to the apps list

.. code-block:: python

    INSTALLED_APPS = [
        'django.contrib.admin',
        'django.contrib.auth',
        'django.contrib.contenttypes',
        'django.contrib.sessions',
        'django.contrib.messages',
        'django.contrib.staticfiles',
        'appname'
    ]

9.) Create a ``templates`` folder in the app folder and create a html file for the app

::

    <body>Hello world!</body>

10.) Create a ``urls.py`` inside the app folder that django will use to look for any URLs that belong to the app

.. code-block:: python

    # general django path function
    from django.urls import path

    # your custom URLs (views.py is already created when the app was setup)
    from appname import views

    # it is a good idea to give each url a "name=" for easier debugging down the road
    urlpatterns = [
        path('',views.hello, name='hello_world'),
    ]

11.) Create the corresponding function view inside the ``views.py`` that pipes the function name to the html file

.. code-block:: python

    from django.shortcuts import render

    # Create your views here. The render method will look for html files within a "templates" directory
    def hello(request):
        # context is data that we can pass to an HTML template
        context = {}
        return render(request, 'hello_world.html', context)

12.) Finally add the path the ``app/urls.py`` into the project ``urls.py``

    - ``path('',`` is the home landing page, the same was as ``path('admin/'`` is the landing page for ``yoursite/admin``

    - ``include('hello_world.urls')`` is which app urls should be piped when the user lands the page

.. code-block:: python
    :emphasize-lines: 5,9

    from django.contrib import admin
    from django.urls import path

    # to hook up your custom URLs
    from django.urls import include

    urlpatterns = [
        path('admin/', admin.site.urls),
        path('', include('hello_world.urls'))
    ]

13.) check that the initial setup was successful ``python manage.py runserver``

Passing Variables to HTML via context
-----------------------------------------
The templating code that Django uses within ``HTML`` is very similar to ``JINJA2``
(see more at `Django Docs <https://docs.djangoproject.com/en/2.2/ref/templates/language//>`_. We can access data
we passed through our ``context dictionary`` like so:

- Let's say our context dictionary contains the following (inside our ``views.py``):

.. code-block:: python

    # this is within our "views.py" file

    def hello(request):
        data = {
            "var1": 1,
            "var2": [10,20,30],
            "var3": {"var3key1": "value1", "var3key2": "value2"}
        }
        data2 = {
            "var1": "data from data2 variable"
        }
        # context is data that we can pass to an HTML template
        context = {
            "data_key": data,
            "data2_key": data2,
        }
        return render(request, 'hello_world.html', context)

- To access a variables in HTML. Note each ``key`` in ``context`` is a direct variable that can be accessed
in the HTML. For demonstration on what name gets used where, the following example will have slight different
names for keys/variables (we would not do this in practice, for simplicity name keys/variables the same).

.. code-block:: html

    <body>
        <p>
            Here is how we call a variable {{ data_key.var1 }} <br>
            Here is how we call a variable within a list {{ data_key.var2.0 }} <br>
            Here is how we call a variable within a dict {{ data_key.var3.varkey1 }} <br>
            Here is how we call another variable {{ data2_key.var1 }} <br>
        </p>
    </body>

- For Loops with variables

.. code-block:: html

    <body>
        <p>
            Here are items from a list:
            {% for item in data_key.var2 %}
                <li>{{ item }}</li>
            {% endfor %}
        </p>
    </body>

- If, elif, else

.. code-block:: html

    <body>
        <p>
            {% if item data_key.var1 == 1 %}
                variable is equal to 1
            {% elif == 2 %}
                variable is equal to 2
            {% else %}
                variable is not equal to anything
            {% endif %}
        </p>
    </body>

Setting Up Project Wide Templates
---------------------------------
To get the same formatting on all your app pages. Anywhere from same layout, fonts, colors etc.

1.) Create a ``templates`` folder within the project folder and create a ``base.html`` file within it
that contains any HTML that is the same for all templates

.. code-block:: html

    <html>
    <head>
        <title> This is a constant title across all html templates </title>
    </head>

    {% block page_content %}{% endblock %}

    </html>

2.) Add the templates HTML file to your django project ``settings.py``

.. code-block:: python
    :emphasize-lines: 4

    TEMPLATES = [
        {
            "BACKEND": "django.template.backends.django.DjangoTemplates",
            "DIRS": ["personal_portfolio/templates/"],
            "APP_DIRS": True,
            "OPTIONS": {
                "context_processors": [
                    "django.template.context_processors.debug",
                    "django.template.context_processors.request",
                    "django.contrib.auth.context_processors.auth",
                    "django.contrib.messages.context_processors.messages",
                ]
            },
        }
    ]

3.) Decorate your app HTML files with the template HTML format

.. code-block:: html


    {% extends "base.html" %}

    {% block page_content %}
    <h1>Hello World!</h1>
    {% endblock %}

Setting Up Static Files
-----------------------
Django uses a folder name ``static`` to look for any static files you may have within your project. These
can be CSS code blocks used for your ``base.html``, images, etc. Here are the steps in setting it up:

1.) Create a ``static`` folder within your project and for this example let's create a subfolder images with a image file

::

    git_repo
       |
       |-projectname
       |  |
       |  |-templates
       |  |  |-base.html
       |  |-__init__.py
       |  |-settings.py
       |  |-urls.py
       |  |-wsgi.py
       |-manage.py
       |-static
          |-images
             |-myimage.png

2.) Add the static file path to your ``setting.py`` file

.. code-block:: python

    STATIC_URL = '/static/'

    STATICFILES_DIRS = [
        os.path.join(BASE_DIR, 'static'),
    ]

3.) Start using the static files within your HTML. The following file is our ``base.html`` but the same
rules applies to all other html files you want to use static files in

.. code-block:: html

    {% load static %}

    <img src="{% static 'image/myimage.png' %}">

Setting Up Admin
----------------
Django sets up a lot of really nice boiler plate website/user/group and more editing via the Admin site.
In order to log into the ``localhost/admin`` site we need the step through the following:

1.) If ``makemigrations`` has not been run to setup a database where users can be stored:

.. code-block:: shell

    python manage.py makemigrations

2.) Makemigrations only prepares the the necessary settings for django, to implement them we need to:

.. code-block:: shell

    python manage.py migrate

3.) Now that the database is setup we can create a superuser

.. code-block:: shell

    python manage.py createsuperuser

4.) To use the superuser, run the server ``python manage.py runserver`` and navigate to ``localhost/admin/``
and log in with your user ID and password

Setting Up Databases
--------------------
To store any kind of data on your website you have to go through a database.  Django uses Structured Query Language (SQL)
under the good as it's database language, however Django has written a Object Relational
Mapper (ORM) that wraps the whole database experience in python code only.

1.) Database interfaces are unique to each app, and the ORM interface in located in the ``model.py`` file
Here is a list of field types for django: `Field Types <https://docs.djangoproject.com/en/2.1/ref/models/fields/#field-types>`_

.. code-block:: python

    from django.db import models

    class BlogData(models.Model):
        title = models.CharField(max_length=100)
        desc = models.TextField()
        group = models.CharField(max_length=20)
        img = models.FilePathField(path="/img")

2.) Using Django's to restructure your ``model.py`` classes into the format it needs to write our SQL code of
the data structure you specified in python code. Note if you get an error: ``No installed app with label 'appname'.``
then you need to add your app to the project TEMPLATES list in the ``settings.py``

- To setup migrations (note you will also need to migrate after this command): ``python manage.py makemigarations appname``
- All subsequent migrations: ``python manage.py migrate appname``

3.) How to add data to our database from django shell

    3.1) Start Django shell: ``python manage.py shell``

    3.2) Import your Database Class and edit/save

    .. code-block:: python

        # where "blog" is the app folder name that has a "models.py" where we defined our "BlogData"
        from blog.models import BlogData

        # create a instance of our Class BlogData
        post1 = BlogData(
            title = 'first title',
            desc = 'first description'
            group = 'group1'
            img = 'img/pic1.png'
        )
        # save it to the database class
        post1.save()

        post2 = BlogData(
            title = 'second title',
            desc = 'second description'
            group = 'group2'
            img = 'img/pic2.png'
        )
        post2.save()

4.) How to access data from our database (be sure to ``python manage.py migrate``) before trying to access
data that you just saved from step 3.

.. code-block:: python

    # to access the database we first need to import it
    from blog.models import BlogData

    # then we can get all items stored
    all_posts = BlogData.objects.all()

    # query just a single post by primarykey (pk)
    post = BlogData.objects.get(pk=1)

    # query by any other file name that we specified
    post = BlogData.objects.get(title='first title')

    # access data from the post
    post.title
    >>> 'first title'
    post.pk # this is the primarykey
    >>> 1
    post.id # this is the primarykey as well