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
    
