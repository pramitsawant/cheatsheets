# Django Cheat Sheet

A cheat-sheet for creating web apps with the Django framework using the Python language. Most of the summaries and examples are based off the [official documentation](https://docs.djangoproject.com/en/2.1/ "official documentation") for Django v2.1.

------------
### Sections
-  [Initializing pipenv](#initializing-pipenv-optional) (optional)
- [Creating a project](#creating-a-project)
  - [Migrate admin module](#migrate-admin-module)
  - [Create superuser](#create-admin-superuser)
- [Creating an app](#creating-an-app)
  - [Register app](#register-your-app)
  - [Create Views](#create-views)
    - [Create a view](#create-a-view)
    - [Link view to url routes](#link-view-to-url-routes)
    - [Connect App Routes to project routes](#connect-app-routes-to-project-routes)
   - [Create Template Views](#create-template-views) 
     - [Create a Template directory](#create-a-template-directory) 
     - [Create a view from template](#create-a-view-from-template)
     - [include static directory path in `settings.py`](#include-static-directory-path-in-settings.py)
  - [Models](#models)
    - [Creating a Model](#creating-a-model)  
    - [Migrate Model](#migrate-model)
    - [Register Model to admin page](#register-model-to-admin-page)
   - [Model queries](#models-queries)
     - [Objects CRUD](#objects-crud)
 - Advance models
------------
### Initializing pipenv (optional)
- Create a project folder and navigate that folder
`mkdir myproject && cd myproject`
- Initialize pipenv with `pipenv install`
- Activate pipenv shell with  `pipenv shell`
- Install django with `pipenv install django`
- Install other package dependencies with  `pipenv install <package_name>`
------------
### Creating a Project
-  Make sure you are in the right directory
- Create project with `django-admin startproject <project_name>`

The project directory should look like this:

```
project/
    manage.py
    project/
        __init__.py
        settings.py
        urls.py
        wsgi.py
```
- Run the development server with `python manage.py runserver` within the project directory

##### Migrate admin module
To migrate changes over run `python manage.py migrate`

##### Create admin superuser
- To create a superuser  `python manage.py createsuperuser` and enter your details such as username, email and password
- go to admin panel http://127.0.0.1:8000/admin/ and login
------------
### Creating an app
- Make sure you are in tha directory which has manage.py file
- Create app with `python manage.py startapp <app_name>`
- Inside the app folder, create a file called urls.py**

#### Register your app
- To include this app in your project, add your app to the project's `settings.py` file by adding its name to the `INSTALLED_APPS` list:
```bash
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'app',
]
```
#### Create Views
##### Create a View
- Within the app directory, open `views.py` and add the following:
```python
from django.http import HttpResponse
def index(request):
    return HttpResponse("Hello, World!")
```
##### Link view to url routes
- Still within the app directory, open (or create) `urls.py` and connect routes to views
```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.index, name='index'),
]
```
##### Connect App Routes to project routes
-   Now within the project directory, edit `urls.py` to include the following
```python
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('app/', include('app.urls')),
    path('admin/', admin.site.urls),
]
```
`Note : The urls.py file within app directories are organized by the urls.py found in the project folder.`

#### Create Template Views
##### Create a Template directory
- Within the app directory, HTML, CSS, and JavaScript files are located within the following locations:
```
app/
   templates/
      index.html
   static/
      style.css
      script.js
```
##### Create a view from template
- To add a template to views, open `views.py` within the app directory and include the following:
 ```python
from django.shortcuts import render

def index(request):
    return render(request,'index.html')
```

-   To include context to the template:
 ```python
def index(request):
	context = {"context_variable": context_variable}
    return render(request,'index.html', context)
 ```
 - Within the HTML file, you can reference static files by adding the following:
 ```html
{% load static %}
<!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="UTF-8">
		<meta name="viewport" content="width=device-width, initial-scale=1">		
		<link rel="stylesheet" href="{% static 'styles.css' %}">
	</head>
	<body>
	{{context_variable}}
    </body>
</html>
```
##### include static directory path in `settings.py`
-   To make sure to include the following in your `settings.py`:
```python
STATIC_URL = '/static/'
STATICFILES_DIRS = [
	os.path.join(BASE_DIR, "static")
]
```
------------
### Models
#### Creating a Model
- Within the app's `models.py` file, an example of a simple model can be added with the following:
```python 
from django.db import models
class Person(models.Model):
	first_name = models.CharField(max_length=30)
	last_name = models.CharField(max_length=30)
```
`Note that you don't need to create a primary key, Django automatically adds an IntegerField.`
#### Migrate Model
- To inact changes in your models, use the following commands in your shell:
```
python manage.py makemigrations <app_name>
python manage.py migrate
```
`Note: including <app_name> is optional.`
#### Register Model to admin page
- To add a model to the Admin page include the following in `admin.py`:
```python
from django.contrib import admin
from .models import Person

admin.site.register(Person)
```
### Models Queries
- To open shell, run the following command:
```python manage.py shell```

- Example `models.py` file:
```python
from django.db import models

class Blog(models.Model):
    name = models.CharField(max_length=100)
    tagline = models.TextField()

    def __str__(self):
        return self.name

class Author(models.Model):
    name = models.CharField(max_length=200)
    email = models.EmailField()

    def __str__(self):
        return self.name

class Entry(models.Model):
    blog = models.ForeignKey(Blog, on_delete=models.CASCADE)
    headline = models.CharField(max_length=255)
    body_text = models.TextField()
    pub_date = models.DateField()
    mod_date = models.DateField()
    authors = models.ManyToManyField(Author)
    n_comments = models.IntegerField()
    n_pingbacks = models.IntegerField()
    rating = models.IntegerField()

    def __str__(self):
        return self.headline
   ```
 ##### Objects CRUD
 ```python
from blog.models import Blog

## Create
b = Blog(name='Beatles Blog', tagline='All the latest Beatles news.')
b.save() 

## Retrieve
all_entries = Entry.objects.all() 	
indexed_entry = Entry.objects.get(pk=1)
find_entry = Entry.objects.filter(name='Beatles Blog')

## Update
entry= Entry.objects.get(pk=1) 
entry.name='Beatles Blog Updated' 
entry.save()

## Delete
entry= Entry.objects.get(pk=1) 
entry.delete()
```
## ORM Queries (Django)

## QuerySets
```python
Person.objects.values('name')
Person.objects.values_list('name')
Person.objects.values_list('name',flat=True)
Person.objects.values_list('name',flat=True).filter(name__contains="p")
```
### Where
```python
Membership.objects.get(id__exact=1)
Membership.objects.get(id__iexact=1)  ##Case Sensitive
```
### Distinct
```python
Membership.objects.distinct('membership_plan')
```
### Like | Contains | Starts with
```python
Person.objects.filter(name__contains="pram")
Person.objects.get(name__icontains='Pram')
Person.objects.filter(name__startswith='Pram')
Person.objects.filter(name__istartswith='Pram') ##Case Sensitive
Person.objects.filter(name__endswith='Lennon')
Person.objects.filter(name__iendswith='Lennon') ##Case Sensitive
```
### IN
```python
Person.objects.filter(id__in=[1, 3])
```
### gt | gte | eq | lt | lte
```python
Person.objects.filter(id__gt=4)
Person.objects.filter(id__gte=4)
Person.objects.filter(id__eq=4)
Person.objects.filter(id__lt=4)
Person.objects.filter(id__lte=4)
```
### Range
```python
import datetime
start_date = datetime.date(2005, 1, 1)
end_date = datetime.date(2005, 3, 31)
Person.objects.filter(pub_date__range=(start_date, end_date))
```
### Date | Year | Month | Day | Week | Quater | Time 
```python
Entry.objects.filter(pub_date__date=datetime.date(2005, 1, 1))
Entry.objects.filter(pub_date__date__gt=datetime.date(2005, 1, 1))

Entry.objects.filter(pub_date__year=2005)
Entry.objects.filter(pub_date__year__gte=2005)

Entry.objects.filter(pub_date__iso_year=2005) ## iso year
Entry.objects.filter(pub_date__iso_year__gte=2005) ## iso year

Entry.objects.filter(pub_date__month=12)  	## Month
Entry.objects.filter(pub_date__month__gte=6)	## Month

Entry.objects.filter(pub_date__day=3)		## Day
Entry.objects.filter(pub_date__day__gte=3)	## Day

Entry.objects.filter(pub_date__week=52)		## Week
Entry.objects.filter(pub_date__week__gte=32, pub_date__week__lte=38) ## Week

Entry.objects.filter(pub_date__week_day=2)	##Weeekday
Entry.objects.filter(pub_date__week_day__gte=2) ##Weeekday

Entry.objects.filter(pub_date__quarter=2)  #Quater

Entry.objects.filter(pub_date__time=datetime.time(14, 30))  ##Time
Entry.objects.filter(pub_date__time__range=(datetime.time(8), datetime.time(17))) ##Time

Event.objects.filter(timestamp__hour=23)	## Hour
Event.objects.filter(time__hour=5)		## Hour
Event.objects.filter(timestamp__hour__gte=12)	## Hour

Event.objects.filter(timestamp__minute=29)	## Minute
Event.objects.filter(time__minute=46)		## Minute
Event.objects.filter(timestamp__minute__gte=29)	## Minute

Event.objects.filter(timestamp__second=31)	## Second
Event.objects.filter(time__second=2)		## Second
Event.objects.filter(timestamp__second__gte=31)	## Second
```
### Null
```python
Entry.objects.filter(pub_date__isnull=True)
```
### Regex
```python
Entry.objects.get(title__regex=r'^(An?|The) +')
Entry.objects.get(title__iregex=r'^(an?|the) +') ## Case sensitive
```

## Aggregation

### Count | Average | Max
```python
Book.objects.count()		## Count
Book.objects.filter(publisher__name='BaloneyPress').count() ## Count

from django.db.models import Avg, Max, FloatField

Book.objects.all().aggregate(Avg('price'))	## Avg

Book.objects.all().aggregate(Max('price'))	## Max

Book.objects.aggregate(price_diff=Max('price', output_field=FloatField()) - Avg('price'))  ## FloatField
```
### Count 
```python
# Each publisher, each with a count of books as a "num_books" attribute.
from django.db.models import Count
pubs = Publisher.objects.annotate(num_books=Count('book'))

# Each publisher, with a separate count of books with a rating above and below 5
from django.db.models import Q
above_5 = Count('book', filter=Q(book__rating__gt=5))
below_5 = Count('book', filter=Q(book__rating__lte=5))
pubs = Publisher.objects.annotate(below_5=below_5).annotate(above_5=above_5)
pubs[0].above_5
pubs[0].below_5

# The top 5 publishers, in order by number of books.
pubs = Publisher.objects.annotate(num_books=Count('book')).order_by('-num_books')[:5]`
```

https://docs.djangoproject.com/en/2.2/ref/models/querysets/#exact
https://docs.djangoproject.com/en/2.2/topics/db/aggregation/












