# Django


??? info "Quick Start"
    - Create a new directory and change to it
    - Create a virtual environment using `python -m venv venv`
    - Activate the virtual env using `venv\Scripts\activate`
    - Install latest Django using `pip install django`
    - Post installation create a django project using `django-admin startproject smartnotes .`
    - To run this boilerplate project just run `python manage.py runserver <port>`
    - The project should start serving on localhost on the port provided

??? info "Expanding the project"
    - Django projects are organized by apps
    - Apps can be created using `django-admin startapp <appname>`
    - This will create a '<appname>' folder in the project which will have multiple files
    - To add a basic view add the final state of the views.py file should be:
        ```python
            from django.shortcuts import render
            from django.http import HttpResponse

            def home(request):
                return HttpResponse('Hello World!')
        ```
    - The request is sent by the urls.py from the root project. Its final state should be:
        ```python
            from django.contrib import admin
            from django.urls import path

            from home import views

            urlpatterns = [
                path('admin/', admin.site.urls),
                path('home', views.home),
            ]
        ```
    - The default localhost page will start returning a 404 now but new end points will be added one of them would be the app name
    - The views are served at that end point, example: `http://127.0.0.1:8001/home`
    - Django uses a framework called MVT i.e. Model view template
    - In order to pass a proper html to the app we need to create a template directory inside the appname directory and create another appname directory inside templates directory
    - In the templates/appname directory add the html file and include the required markup
    - Change the views.py file to render this html page by returning the render function output: `render(request, 'appname/home.html', {'today', datetime.today()})`
    - The braces at the end is a way to pass on information to the view which can be accessed on the front end using `{{{today}}}`
    - Django projects are designed to be modular. The apps should be organized in a way that even if one app is deleted the project should not go down.
    - In order to do this apps should be self contained and have just 1 concern
    - Best practice is to have urls.py in each app which handles the routing and use include function in the project urls.py

??? info "Built-in user management"

    - Django Admin:
        - To activate the admin interface you do not need to do anything special. Admin is alreay implemented.
        - You need to create a superuser though to access it.
        - First migrate the database using `python manage.py migrate`
        - Then simply run `python manage.py createsuperuser` to create a super user. The app will ask questions and create a user based on responses
        - Access admin panel using the credentials
        - By default the django admin provides way to create users and specify passwords and set previliges
        - Any changes here is stored in the database
    
    - Simple user authentication based access
        - In order to restrict access to authorized users we can simply do the below in views.py:
            ```python
                from django.contrib.auth.decorators import login_required
                @login_required(login_url='/admin')
                def authorized(request):
                    return render(request, 'home/auth.html', {})
            ```
        - In addition to the view we need to update the view in the urls.py:
            ```python
                urlpatterns = [
                    path('home', views.home),
                    path('authorized', views.authorized),
                ]
            ```
        - That's about it
    
??? info "Connecting databases"

    - Creating the ORM model
        - The sequence of flow that Django follows to create a model is: 
            - Create ORM model in models.py of the app
            - Run `Makemigrations`
            - Run `migrations`
        - ORMs basically allow us to configure a object oriented model of the RDBMs
        - In the models.py define a class where the class name should be the name of the table & attribute to be name of the columns
        - A typical class can be:
        ```python
        from django.db import models

        # Create your models here.
        class Notes(models.Model):
            title = models.CharField(max_length=200)
            note = models.TextField()
            created = models.DateTimeField(auto_now_add=True)
        ```
        - After creating this run `python manage.py makemigrations` to create the migrations
        - After this run `python manage.py migrate` to migrate the ORMs

    - Exposing the model via Django Admin:
        - Choose the admin.py from the app
        - Override the `admin.ModelAdmin` class with the ModelAppname class
        - either return a pass in case no extra customization is needed or make customizations
        - Register this admin 
        - The sample class is as follows:
            ```python
            from django.contrib import admin

            from . import models

            class NotesAdmin(admin.ModelAdmin):
                list_display = ('title',)
            # Register your models here.

            admin.site.register(models.Notes, NotesAdmin)
            ```
        - Once saved the model will be available from the django admin menu
    
    - Adding and querying data from Django shell:
        - Invoking Django shell: `python manage.py shell`
        - Import the model for the app e.g. `from notes.model import Notes`
        - In the shell use `notes = Notes.objects.all()` to get a QueryList of all notes in the db
        - In the shell use `new_note = Notes.objects.create(title='', note='')` to create a new note
        - In the shell use `Notes.objects.filter(title__startswith = 'My')` to filter during querying

??? info "Building dynamic webpages"

    - Showing data from the database on html pages:
        - We need to make changes to views.py and urls.py in the app
        - We need to change the urls.py from the project
        - We need to add a template html file
    
    Example code: appname/views.py
    ```python
    from django.shortcuts import render
    from .models import Notes
    # Create your views here.

    def list(request):
        list_notes = Notes.objects.all()
        return render(request, 'notes/smart_notes.html', {'notes':list_notes})
    ```

    Example code: appname/urls.py
    ```python
    from django.urls import path
    from . import views

    urlpatterns = [
        path('notes', views.list)
    ]   
    ```
    Example html code to read data:
    ```html
    <body>
        <ul>
            {% for note in notes %}
                <li>{{note.title}}</li>
            {% endfor %}
        </ul>
    </body>
    ```

    ==Class Based Views:==
        - Class based views are alternative way to create views:
        - Example class view:
        ```python
        from django.contrib.auth.mixins import LoginRequiredMixin
        from django.views.generic import TemplateView

        # simple class view
        class HomeView(TemplateView):
            template_name = 'home/welcome.html'
            extra_context = {'today': datetime.today()}

        # authorized view
        class AuthorizedView(LoginRequiredMixin, TemplateView):
            template_name = 'home/auth.html'
            login_url = '/admin'
        ```

??? info "Building a front end"

    - Setting up static directory:
        - Create a directory called "static" on the root level
        - You need to tell Django that this will host the static files
        - The way to do it is the settings.py
        - The settings.py should have the following setup:
        
            ```python
            STATIC_URL = 'static/'

            # add this
            STATICFILES_DIRS = [
                BASE_DIR / 'static',
            ]
            ```
        - In order to invoke the contents of this directory in the template htmls we need to do following:
            ```html
            {% extends "base.html" %}

            {% block content %}
            <ul>
                {% for note in notes %}
                    <li class="note-li">{{note.title}}</li>
                {% endfor %}
            </ul>
            {% endblock %}
            ```
        - Where the base html can be:
            ```html
            {% load static %}
            <!DOCTYPE html>
            <html lang="en">
            <head>
                <meta charset="UTF-8">
                <meta name="viewport" content="width=device-width, initial-scale=1.0">
                <link rel="stylesheet" href="{% static 'css/style.css' %}">
                <title>Basecamp</title>
            </head>
            <body>
                {%block content%}
                {%endblock%}
            </body>
            </html>
            ```