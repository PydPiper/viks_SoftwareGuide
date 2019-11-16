lib - django (web framework)
============================


Quick Setup Guide
-----------------

1.) Create a git repo and clone it down

2.) Create a venv within the repo and activate the venv

3.) ``pip install django``

4.) ``django-admin startproject projectname``

5.) restructure folder

6.) check that the initial setup was successful ``python manage.py runserver``

7.) create a new app ``python manage.py startapp appname``

    7.1) ``admin.py`` is your django admin settings

    7.2) ``apps.py`` is your app settings

    7.3) ``models.py`` is your database tables

    7.4) ``test.py`` test cases

    7.5) ``views.py`` settings for what will be displayed as HTML

8.) after a new app is created it must be added to the ``settings.py`` > ``INSTALLED_APPS`` list
It is the directory name of the app that is to be added to the apps list

9.) Create a ``templates`` folder in the app folder and create a html file for the app

10.) Create a ``urls.py`` inside the app folder that django will use to look for any URLs that belong to the app

.. code-block:: python

    # general django path function
    from django.urls import path

    # your custom URLs (views.py is already created when the app was setup)
    from appname import views

    urlpatterns = [
        path('',views.hello, name='hello_world'),
    ]

11.) Create the corresponding function view inside the ``views.py`` that pipes the function name to the html file

.. code-block:: python

    from django.shortcuts import render

    # Create your views here. The render method will look for html files within a "templates" directory
    def hello(request):
        return render(request, 'hello_world.html', {})

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


Setting Up Project Wide Templates
---------------------------------

1.) Create a ``templates`` folder within the project folder and create a ``base.html`` file within it that contains the following

.. code-block:: html

    {% block page_content %}{% endblock %}

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