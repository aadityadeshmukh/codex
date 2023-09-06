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
        - External CSS also can be used like Bootstrap or Tailwind

??? info "CRUD - remaining parts"

    Until now we have dealt with the Read part of the CRUD operations.
    We will not deal with the CREATE, UPDATE AND DELETE parts

    CREATE:

    - We will need to add a create end point
    - Use the following code in views.py:
    ```python
    from django.views.generic import CreateView, DetailView, ListView
    # Create your views here.

    class NoteCreateView(CreateView):
        model = Notes
        fields = ['title', 'note']
        success_url = '/smart/notes'
    ```
    - urls.py:
    ```python
    from django.urls import path
    from . import views

    urlpatterns = [
        path('notes', views.NoteListView.as_view(), name='notes.list'),
        path('notes/<int:pk>', views.NoteDetailView.as_view(), name='notes.detail'),
        path('notes/new' , views.NoteCreateView.as_view(), name='notes.new')
    ]
    ```
    - Template html:
    ```html
    <!-- notes_form.html -->
    {% extends "base.html" %}

    {%block content%}

    <form action="{% url 'notes.new'%}" method="POST">{% csrf_token %}
        {{form}}
        <button type="submit" class="btn btn-primary my-5">Submit</button>
    </form>
    {%endblock%}
    ```
    - Form validations are an important part and Django makes it easy to work with them
    - Create a forms.py file in the app and add the code as illustrated below:
    ```python
    from django import forms
    from django.core.exceptions import ValidationError

    from .models import Notes

    class NotesForm(forms.ModelForm):
        class Meta:
            model = Notes
            fields = ('title', 'note')
            widgets = {
                'title': forms.TextInput(attrs={'class' : 'form-control my-5'}),
                'note': forms.Textarea(attrs={'class' : 'form-control mb5'})
            }
            labels = {
                'note': "What's on your mind?"
            }
        
        def clean_title(self):
            title = self.cleaned_data['title']
            if 'Django' not in title:
                raise ValidationError('Not a django related note')
            return title
    ```
    - The above class needs to be simply included in the views.py in the create view section
    - The above also adds ways to control UX elements like label and css injection using labels and widgets dictionary
    ```python
        class NoteCreateView(CreateView):
        model = Notes
        # fields = ['title', 'note']
        success_url = '/smart/notes'
        form_class = NotesForm
    ```
    - The form errors can be exluded by using css selectors and hiding them
    - They can also be styled by adding the below code to the template:
    ```python
        {% if form.errors %}
            <div class="alert alert-danger my-5">
                {{form.errors.title.as_text}}
            </div>
        {%endif%}
    ```

    ==UPDATE==

    - Updating is really easy as it is a natural extension of the create part.
    - First add the UpdateView in views.py:
        ```python
        from django.views.generic import CreateView, DetailView, ListView, UpdateView
        # Create your views here.

        class NoteUpdateView(UpdateView):
            model = Notes
            # fields = ['title', 'note']
            success_url = '/smart/notes'
            form_class = NotesForm
        ```
    - Update the NoteUpdateView in urls.py which will add an edit end point
        ```python
        path('notes/<int:pk>/edit', views.NoteUpdateView.as_view(), name='notes.update'),
        ```
    - This will basically add the functionality update the fields.
    - We can beautify it by adding relevant buttons to the templates
    - For example and edit button on the detail page `<a href="{% url 'notes.update' pk=note.id %}" class="btn btn-secondary">Edit</a>`
    - And cancel button on the edit page: `<a href="{% url 'notes.list' %}" class="btn btn-secondary">Cancel</a>`

    ==DELETE==

    - Deletion is quite similar to update.
    - Create a DeleteView in views and include it in urls.py
    ```python
    from django.views.generic.edit import DeleteView
    from .forms import NotesForm
    # Create your views here.

    class NoteDeleteView(DeleteView):
        model=Notes
        success_url = '/smart/notes'
        template_name = 'notes/notes_delete.html'
    ```
    - Inclusion for urls.py
    ```python
    path('notes/<int:pk>/delete', views.NoteDeleteView.as_view(), name='notes.delete'),
    ```
    - Add a template to specify a confirmation message.
    - Easy peasy.
    ```html
    {%extends "base.html" %}

    {%block content%}
    <form method="POST">{% csrf_token %}
        <p>Are you sure you want to delete "{{notes.title}}"?</p>
        <input type="submit" class="btn btn-danger" value="confirm">
    </form>
    {%endblock%}
    ```

??? info "User management"
    - Django already has a user database with one user admin with pk=1
    - We first need to create migrations and run them to associate all notes to the admin user
    - This will also make the app user aware i.e. we should be able to show only those notes to users who created them
    - First update the model:
    ```python
        from django.db import models
        from django.contrib.auth.models import User

        # Create your models here.
        class Notes(models.Model):
            title = models.CharField(max_length=200)
            note = models.TextField()
            created = models.DateTimeField(auto_now_add=True)
            user = models.ForeignKey(User, on_delete=models.CASCADE, related_name="notes")
    ```
    - Once done create migrations using `python manage.py makemigrations`
    - Then run the migration `python manage.py migrate`
    - It will ask for a default user. Enter 1 i.e. pk for admin
    - This will associate all existing notes with the admin user
    - Next update the views that need logins for example the list view:
    ```python
    from django.contrib.auth.mixins import LoginRequiredMixin
    # Code before ---
    class NoteListView(LoginRequiredMixin, ListView):
    model = Notes
    context_object_name = "notes"
    template_name = 'notes/smart_notes.html'
    login_url = '/admin'

    def get_queryset(self):
        return self.request.user.notes.all()

    # Code after ---
    ```
    - 