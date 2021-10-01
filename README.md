**Table of Content**
- [Introduction](#introduction)
  - [Creating django project](#creating-django-project)
  - [Create database](#create-database)
  - [Run django project](#run-django-project)
  - [Create an app](#create-an-app)
  - [Creating super user](#creating-super-user)
- [MVC Architect](#mvc-architect)
  - [Models](#models)
    - [Some useful fields](#some-useful-fields)
    - [Some useful parameters](#some-useful-parameters)
      - [null](#null)
      - [blank](#blank)
      - [default](#default)
      - [choices](#choices)
    - [Relationships](#relationships)
      - [OneToOne](#onetoone)
      - [OneToMany or ManyToOne](#onetomany-or-manytoone)
      - [ManyToMany](#manytomany)
      - [On delete parameters](#on-delete-parameters)
    - [User model](#user-model)
      - [login](#login)
      - [password change](#password-change)
      - [logout](#logout)
      - [Authentication](#authentication)
      - [permission](#permission)
    - [Custom User](#custom-user)
    - [Getting and manipulating models](#getting-and-manipulating-models)
      - [Object-Relational Mapping](#object-relational-mapping)
      - [Create object with model](#create-object-with-model)
      - [Read](#read)
        - [Some useful QuerySet methods](#some-useful-queryset-methods)
        - [Inverse relationship](#inverse-relationship)
        - [Aggregate methods](#aggregate-methods)
      - [Update](#update)
      - [Delete](#delete)
    - [Functions in models](#functions-in-models)
    - [Manager](#manager)
      - [Expanding manager](#expanding-manager)
  - [Views](#views)
    - [Make modular URLs](#make-modular-urls)
    - [Dynamic URLs](#dynamic-urls)
    - [POST request](#post-request)
      - [Postman](#postman)
    - [Object-Oriented View](#object-oriented-view)
      - [Function Based View(FBV)](#function-based-viewfbv)
      - [Class Based View(CBV)](#class-based-viewcbv)
    - [Generic Views](#generic-views)
      - [ListView](#listview)
        - [Pagination](#pagination)
        - [Filtering](#filtering)
      - [DetailView](#detailview)
        - [Filtering](#filtering-1)
      - [CreateView](#createview)
        - [Initial values](#initial-values)
      - [UpdateView](#updateview)
      - [FormView](#formview)
      - [DeleteView](#deleteview)
    - [Mixin Class](#mixin-class)
  - [Templates](#templates)
    - [Static files(CSS and JS files)](#static-filescss-and-js-files)
    - [Variables](#variables)
    - [Tags](#tags)
      - [Condition](#condition)
      - [Loops](#loops)
      - [Inheritance](#inheritance)
    - [Filters](#filters)
      - [lower](#lower)
      - [length](#length)
      - [filesizeformat](#filesizeformat)
      - [truncatedwords](#truncatedwords)
      - [default](#default-1)
      - [chained filters](#chained-filters)
  - [Forms](#forms)
    - [Validation](#validation)
      - [errors](#errors)
      - [custom validation](#custom-validation)
    - [Form in view](#form-in-view)
      - [Turning form to object](#turning-form-to-object)
    - [Form in template](#form-in-template)
    - [ModelForm](#modelform)
  - [Admin](#admin)
    - [Object representation](#object-representation)
    - [Admin Personalization](#admin-personalization)
      - [list_display](#list_display)
      - [list_display_links](#list_display_links)
      - [sortable_by](#sortable_by)
      - [list_filter](#list_filter)
      - [list_editable](#list_editable)
      - [search_fields](#search_fields)
      - [inlines](#inlines)
      - [fields](#fields)
      - [readonly_fields](#readonly_fields)
      - [extra](#extra)
      - [fieldsets](#fieldsets)
      - [custom actions](#custom-actions)
- [Django Rest Framework(DRF)](#django-rest-frameworkdrf)
  - [Installation](#installation)
  - [Views](#views-1)
    - [Function Based View(FBV)](#function-based-viewfbv-1)
    - [Class Based View(CBV)](#class-based-viewcbv-1)
  - [Serializers](#serializers)
    - [Serializer Class](#serializer-class)
    - [ModelSerializer Class](#modelserializer-class)
  - [Authentication with Token](#authentication-with-token)
    - [Login](#login-1)
    - [Logout](#logout-1)
  - [Permissions](#permissions)
    - [Famouse built-in permission](#famouse-built-in-permission)
    - [Types of permissions](#types-of-permissions)
      - [has_permission](#has_permission)
      - [has_object_permission](#has_object_permission)
  - [CORS(Cross-Origin Resource Sharing)](#corscross-origin-resource-sharing)
    - [corsheaders](#corsheaders)
- [Testing](#testing)
  - [Model Testing](#model-testing)
  - [View Testing](#view-testing)
    - [Testing API](#testing-api)
  - [Test Coverage](#test-coverage)
    - [Installation](#installation-1)
    - [Run the Coverage](#run-the-coverage)
    - [Result of Coverage](#result-of-coverage)
      - [Terminal](#terminal)
      - [HTML](#html)
  - [Fixtures](#fixtures)
  - [Django Debug Toolbar](#django-debug-toolbar)
    - [Installation](#installation-2)

# Introduction
## Creating django project
```
django-admin startproject mysite
```
## Create database
```python 
python manage.py migrate
```
## Run django project
```
python manage.py runserver
```
visit [localhost:8000](localhost:8000)
## Create an app
```
python manage.py startapp first_app
```
Add this app at the main app settings.py file inside INSTALLED_APPS:
```python
INSTALLED_APPS = [
	 'django.contrib.admin',
	 'django.contrib.auth',
	 'django.contrib.contenttypes',
	'django.contrib.sessions',
	 'django.contrib.messages',
	 'django.contrib.staticfiles',
	 'transaction',
]
```
## Creating super user
```python
python manage.py createsuperuser
```
To get changes in database and create things, run this:
```python
python manage.py makemigrations first_app
python manage.py migrate
```
# MVC Architect
## Models
In your app, add a model in models.py:
```python
from django.db import models

class User(models.Model):
	username = models.CharField(max_length=10)
	email = models.EmailField()
	signing_date = models.DateTimeField()
```
### Some useful fields
-   BinaryField
-   BooleanField
-   CharField
-   DateField
-   EmailField
-   DateTimeField
-   IntegerField
-   FloatField
-   TextField
### Some useful parameters
#### null
if it's true, the parameter could be null
#### blank
if it's true, the parameter could be ""
#### default
if it's empty, defualt value will be assigned
#### choices
Create choices in model and use it as parameter:
```python 
GENDER_CHOICES = ( ("m", "male"), ("f", "female"), )
```
### Relationships
#### OneToOne
```python
from django.db import models

class User(models.Model):
	username = models.CharField(max_length=10)
	email = models.EmailField()
	signing_date = models.DateTimeField()
	
class Post(models.Model):
	content = models.TextField()
	writers = models.OneToOneField(User)
```
**Example:** Custom user to user(Only one custom user is attached to the user)
#### OneToMany or ManyToOne
```python
from django.db import models

class User(models.Model):
	username = models.CharField(max_length=10)
	email = models.EmailField()
	signing_date = models.DateTimeField()
	
class Post(models.Model):
	content = models.TextField()
	writers = models.ForeignKey(User, on_delete=models.CASCADE)
```
**Example:** Blog to Writer(One writer can have many blogs)
#### ManyToMany
```python
from django.db import models

class User(models.Model):
	username = models.CharField(max_length=10)
	email = models.EmailField()
	signing_date = models.DateTimeField()
	
class Post(models.Model):
	content = models.TextField()
	writers = models.ManyToManyField(User)
```
**Example:** Blog to Writers(Writers can have many blogs)
#### On delete parameters
-   CASCADE: delete all it's objects
-   PROTECT: can not be deleted if it has an object
-   SET_NULL: set null to it's objects
-   SET_DEFAULT: set default to it's objects
-   SET(...): set this to it's objects
-   DO_NOTHING: nothing happens
### User model
```python
# models.py
from django.db import models
from django.contrib.auth.models import User

class Profile(models.Model):
    user = models.OneToOneField(User, on_delete=models.CASCADE)
    phone_number = models.CharField(max_length=50, blank=True, null=True)
    country = models.CharField(max_length=100)
```
```python
# forms.py
from django import forms
from django.contrib.auth.forms import UserCreationForm
from django.contrib.auth.models import User

from .models import Profile

class SignUpForm(UserCreationForm):
    country = forms.CharField(max_length=100)

    class Meta:
        model = User
        fields = ('username', 'country', 'password1', 'password2')

    def save(self, commit=True):
        user = super().save(commit=commit)
        Profile.objects.create(user=user, country=self.data['country'])
        return user
```
#### login
```python
from django.contrib.auth import authenticate, login as dj_login
from django.http.response import HttpResponse
from django.views.decorators.csrf import csrf_exempt

@csrf_exempt
def login(request):
    if request.method == 'POST':
        username = request.POST['username']
        password = request.POST['password']

        user = authenticate(request, username=username, password=password)
        if user:
            dj_login(request, user)
            return HttpResponse('Login completed!')
        return HttpResponse('Wrong password/username')

    return HttpResponse('Please login with post method')
```
#### password change
```python
# views.py
@csrf_exempt
def change_password(request):
    if request.method == 'POST':
        if not request.user.is_authenticated:
            return HttpResponse('Please login first')

        old_password = request.POST['old_password']
        new_password1 = request.POST['new_password1']
        new_password2 = request.POST['new_password2']

        if not request.user.check_password(old_password):
            return HttpResponse('Wrong old password')

        if new_password1 != new_password2:
            return HttpResponse('Entered passwords are not identical')

        request.user.set_password(new_password1)
        request.user.save()

        return HttpResponse('Password changed successfully!')

    return HttpResponse('Only post method allowed')
```
#### logout
```python
from django.contrib.auth import logout as dj_logout
from django.http.response import HttpResponse
from django.views.decorators.csrf import csrf_exempt

@csrf_exempt
def logout(request):
    if request.method == 'POST':
        if not request.user.is_authenticated:
            return HttpResponse('Please login first')

        dj_logout(request)
        return HttpResponse('Logout successfully')

    return HttpResponse('Only post method allowed')
```
These methods are also available:
[https://developer.mozilla.org/en-US/docs/Learn/Server-side/Django/Authentication](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Django/Authentication)
#### Authentication
```python
from django.contrib.auth.decorators import login_required

def login_first(request):
    return HttpResponse('Please login first')

@csrf_exempt
@login_required(login_url='/library/login-first/')
def logout(request):
    if request.method == 'POST':
        dj_logout(request)
        return HttpResponse('Logout successfully')

    return HttpResponse('Only post method allowed')
```
#### permission
**add**
```python
from django.contrib.auth.decorators import login_required, permission_required

from .forms import SignUpForm, BookForm

@permission_required('library.add_book', raise_exception=True)
@login_required(login_url='/library/login-first/')
@csrf_exempt
def add_book(request):
    if request.method == 'POST':
        form = BookForm(request.POST)
        if form.is_valid():
            form.save()
            return HttpResponse('Book added successfully')

        return HttpResponse(f"{form.errors}")

    return HttpResponse('Only post method allowed')
```
for granting permission using shell, visit this:
[https://docs.djangoproject.com/en/3.0/topics/auth/default/#permissions-and-authorization](https://docs.djangoproject.com/en/3.0/topics/auth/default/#permissions-and-authorization)
**change**
```python
# views.py
from django.shortcuts import get_object_or_404
from django.contrib.auth.decorators import login_required, permission_required

from .forms import SignUpForm, BookForm
from .models import Book

@permission_required('library.change_book', raise_exception=True)
@login_required(login_url='/library/login-first/')
@csrf_exempt
def change_book(request, book_id):
    if request.method == 'POST':
        book = get_object_or_404(Book, id=book_id)
        form = BookForm(request.POST, instance=book)
        if form.is_valid():
            form.save()
            return HttpResponse('Book information updated!')
        return HttpResponse(f"{form.errors}")

    return HttpResponse('Only post method allowed!')
```
```python
# urls.py
from library.views import change_book

urlpatterns = [
    # ... 
    path('library/change-book/<int:book_id>/', change_book),
]
```
**view**
```python
# views.py
from django.shortcuts import get_object_or_404
from django.contrib.auth.decorators import login_required, permission_required
from django.forms.models import model_to_dict
from django.views.decorators.csrf import csrf_exempt

from .models import Book

@permission_required('library.view_book', raise_exception=True)
@login_required(login_url='/library/login-first/')
@csrf_exempt
def view_book(request, book_id):
    if request.method == 'GET':
        book = get_object_or_404(Book, id=book_id)
        return HttpResponse(f"{model_to_dict(book)}")
    return HttpResponse('Only get method allowed!')
```
```python
# urls.py
from library.views import view_book

urlpatterns = [
    # ... 
    path('library/view-book/<int:book_id>/', view_book),
]
```
**delete**
```python
# views.py
@permission_required('library.delete_book', raise_exception=True)
@login_required(login_url='/library/login-first/')
@csrf_exempt
def delete_book(request, book_id):
    if request.method == 'DELETE':
        book = get_object_or_404(Book, id=book_id)
        book.delete()
        return HttpResponse('Book deleted successfully!')
    return HttpResponse('Only delete method allowed!')
```
```python
# urls.py
from library.views import delete_book

urlpatterns = [
    # ... 
    path('library/delete-book/<int:book_id>/', delete_book),
]
```
**custom permissions**
```python
# models.py
class Book(models.Model):
    title = models.CharField(max_length=200)
    publish_date = models.DateField()
    pages_count = models.IntegerField()
    author = models.ForeignKey(
        Author, related_name='books', on_delete=models.CASCADE
    )
    hidden = models.BooleanField(default=False)

    class Meta:
        permissions = [
            ("hide_book", "Can make book hidden"),
        ]

    def __str__(self):
        return self.title
```
### Custom User
```python
from django.db import models
from django.contrib.auth.models import AbstractUser

class CustomUser(AbstractUser):
    email = models.EmailField(unique=True)
    country = models.CharField(max_length=100)
    phone_number = models.CharField(max_length=50, blank=True, null=True)
```
Now you should replace this user with the default; So in the [settings.py](http://settings.py) add this:
```python
AUTH_USER_MODEL = 'library.CustomUser'
```
Now change the admin file:
```python
from django.contrib import admin
from django.contrib.auth.admin import UserAdmin as DefaultUserAdmin

from .models import CustomUser

@admin.register(CustomUser)
class UserAdmin(DefaultUserAdmin):
    fieldsets = (
        (None, {'fields': ('username', 'password')}),
        ('Personal info', {
            'fields': (
                'first_name',
                'last_name',
                'email',
                'phone_number',
                'country'
            )
        }),
        ('Permissions', {
            'fields': (
                'is_active',
                'is_staff',
                'is_superuser',
                'groups',
                'user_permissions'
            ),
        }),
        ('Important dates', {'fields': ('last_login', 'date_joined')}),
    )

    list_display = (
        'username',
        'email',
        'first_name',
        'last_name',
        'phone_number',
        'is_staff',
    )

    search_fields = (
        'username',
        'first_name',
        'last_name',
        'phone_number',
        'email',
    )
```
### Getting and manipulating models
#### Object-Relational Mapping
ORM will map your code to database query! So you won't need to write query codes!
To get and manipulate objects with models, you can use manage.py shell:
```
python manage.py shell
```
You can even run django shell on ipython or bpython:
```
python manage.py shell -i ipython
```
Now you can run your python codes.
#### Create object with model
```python
from first_app.models import *
User.objects.create(name="user")
```
You can also create an object; Then save it to database:
```python
from first_app.models import *
user = User(name="user")
user.save()
```
Another way:
```python
from first_app.models import *
user = User()
user.name = "user"
user.save()
```
#### Read
QuerySet is number of objects related to the model that acts like list. It can have functions and calling them will return another QuerySet. So you can do multiple filtering!
##### Some useful QuerySet methods
- all()
```python
from first_app.models import *
User.objects.all()
```
- filter()
```python
from first_app.models import *
User.objects.filter(score=10)
```
**exact and iexact**
```python
from first_app.models import *
Post.objects.filter(writer__username__exact='sajjad')
```
You can also write it without exact phrase(the default is exact):
```python
from first_app.models import *
Post.objects.filter(writer__username='sajjad')
```
If you want the lookup to be non-sensetive to upper or lower case, use iexact.
**contains and icontains**
```python
from first_app.models import *
Post.objects.filter(content__contains='sajj')
```
If you want the lookup to be non-sensetive to upper or lower case, use icontains.
**lt**
```python
from first_app.models import *
User.objects.filter(score__lt=3) # refers to lower than
```
**lte**
```python
from first_app.models import *
User.objects.filter(score__lte=3) # refers to lower than or equal
```
**gt**
```python
from first_app.models import *
User.objects.filter(score__gt=3) # refers to greater than
```
**gt**
```python
from first_app.models import *
User.objects.filter(score__gte=3) # refers to greater than or equal
```
**startswith and isstartswith**
```python
from first_app.models import *
User.objects.filter(username__startswith='ad')
```
If you want the lookup to be non-sensetive to upper or lower case, use isstartswith.
**endswith and isendswith**
```python
from first_app.models import *
User.objects.filter(username__endswith='n')
```
If you want the lookup to be non-sensetive to upper or lower case, use isendswith.
**in**
```python
from first_app.models import *
User.objects.filter(id__in=[1, 12, 14, 21])
```
- get()
If you want just one object, you can use get:
```python
from first_app.models import *
User.objects.get(name="user")
```
- exclude()
```python
from first_app.models import *
User.objects.exclude(score=10)
```
- order_by()
```python
from first_app.models import *
User.objects.order_by('-score') # reverse order
```
- count()
```python
from first_app.models import *
User.objects.count()
User.objects.filter(age=18).count()
```
- values()
```python
from first_app.models import *
Blog.objects.filter(name='Beatles').values() # values in json format
Article.objects.all().values()
```
you can specify fields in values():
```python
from first_app.models import *
Article.objects.all().values("title")
```
- values_list()
It's like values, but output is a list of tuples:
```python
from first_app.models import *
Entry.objects.values_list('id', 'headline')
```
If it's just one field, you can use flat parameter to make a list:
```python
from first_app.models import *
Entry.objects.values_list('id', flat=True).order_by('id')
```
- More Complex Queries with F and Q
Using F, we can compare two fields. For example we want students who their math score is greater than physics score:
```python
from first_app.models import *
from django.db.models import F
Student.objects.filter(math_score__gt=F('physics_score'))
```
Another way:
```python
from first_app.models import *
from django.db.models import F
physics_score = F('physics_score')
Student.objects.filter(math_score__gt=physics_score)
```
We can even use arithmetic operations:
```python
from first_app.models import *
from django.db.models import F
Student.objects.filter(math_score__gt=F('physics_score')*2)
```
Q object do logical comparison. For example we want students who their math or physics score is above 10:
```python
from first_app.models import *
from django.db.models import Q
Student.objects.filter(Q(math_score__gte=10) | Q(physics_score__gte=10))
```
Another way:
```python
from first_app.models import *
from django.db.models import Q
conditions = Q(math_score__gte=10) | Q(physics_score__gte=10)
Student.objects.filter(conditions)
```
**We show 'and' with &, 'or' with | and 'not' with ~**
For example without Q:
```python
from first_app.models import *
Student.objects.exclude(math_score=10)
```
With Q:
```python
from first_app.models import *
from django.db.models import Q
Student.objects.filter(~Q(math_score=10))
```
For example without Q:
```python
from first_app.models import *
Student.objects.exclude(math_score__in=[10, 13])
```
Another way:
```python
from first_app.models import *
Student.objects.exclude(math_score=10).exclude(math_score=13)
```
With Q:
```python
from first_app.models import *
from django.db.models import Q
Student.objects.exclude(math_score=10).exclude(math_score=13)
```
##### Inverse relationship
You can get a book model's user:
```python
from first_app.models import *
book = Book.objects.get(name="clean_code")
print(book.author)
```
##### Aggregate methods
- Average
```python
from first_app.models import *
from django.db.models import Avg
User.objects.aggregate(Avg('score'))
User.objects.aggregate(average_score=Avg('score')) # naming
```
- Count
```python
from first_app.models import *
from django.db.models import Count
User.objects.aggregate(scores=Count('score'))
```
- Maximum
```python
from first_app.models import *
from django.db.models import Max
User.objects.aggregate(maximum=Max('score'))
```
- Minimum
```python
from first_app.models import *
from django.db.models import Min
User.objects.aggregate(minimum=Min('score'))
```
- Sum
```python
from first_app.models import *
from django.db.models import Sum
User.objects.aggregate(sum=Sum('score'))
```
- Multiple aggregation
```python
from first_app.models import *
from django.db.models import Min, Max, Avg
User.objects.aggregate(minimum=Min('score'), maximum=Max('score'), average=Avg('score'),)
```
#### Update
```python
from first_app.models import *
user = User.objects.get(name="user")
user.update(name="test")
```
#### Delete
```python
from first_app.models import *
user = User.objects.get(name="user")
user.delete()
```
### Functions in models
You can add custom function in models.
```python
from django.db import models
import jdatetime

class Article(models.Model):
	 title = models.CharField(max_length=128, null=False, blank=False)
	 cover = models.FileField(upload_to='files/article_cover', null=True, blank=True, validators=[validate_file_extension])
	 content = RichTextField()
	 createdDate = models.DateTimeField(default=datetime.now, blank=False)
	 createdDateFa = models.CharField()
	 category = models.ForeignKey('Category', on_delete=models.CASCADE)
	 author = models.ForeignKey(CustomUser, on_delete=models.CASCADE)
	 def dateConvertor(self):
		newDate = jdatetime.date.fromgregorian(day=self.createdDate.date().day,month=self.createdDate.date().month,year=self.createdDate.date().year)
		self.createdDateFa = str(newDate)
		self.save()
```
### Manager
```python
from first_app.models import *
User.objects.all()
```
objects is a default manager for models; But you can change the name:
```python
from django.db import models

class Mobile(models.Model):
	price = models.PositiveIntegerField(default=1000)
	mobiles = models.Manager()
```
Now you can use mobiles name:
```python
from first_app.models import *
Mobile.mobiles.all()
```
#### Expanding manager
```python
from django.db import models

class BookManager(models.Manager):
	def sort_by_rate(self):
		return self.order_by('-rate')
	
	class Book(models.Model):
		name = models.CharField(max_length=10)
		rate = models.IntegerField(default=0)
		free = models.BooleanField(default=True)
		objects = BookManager()
```
## Views
```python
from django.http import HttpResponse

def welcome(request):
	return HttpResponse('<h1>Hello World</h1>')
```
To see the respose, you should use a url. So add this to urls.py(also known as ROOT_URLCONF):
```python
from django.contrib import admin
from django.urls import path
from my_app.views import welcome

urlpatterns = [ path('admin/', admin.site.urls), path('say/welcome/', welcome), ]
```
Now visit [http://localhost:8000/say/welcome/](http://localhost:8000/say/welcome/)
### Make modular URLs
make urls.py in your app:
```python
from django.urls import path
from my_app.views import welcome

urlpatterns = [ path('say/welcome/', welcome), ]
```
Now in the ROOT_URLCONF:
```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [ path('admin/', admin.site.urls), path('', include('my_app.urls')), ]
```
### Dynamic URLs
For taking argument with url you need to use <type:name> in the path:
```python
from django.urls import path
from .views import index

urlpatterns = [ path('<str:first_name>/<str:last_name>/<int:age>/', index), ]
```
Then you can take it in the views.py:
```python
from django.http import HttpResponse

def index(request, first_name, last_name, age):
	response = f"name: {first_name} {last_name} | age: {age}"
	return HttpResponse(response)
```
Now visit this:
[http://localhost:8000/Meisam/Taghizadeh/25/](http://localhost:8000/Meisam/Taghizadeh/25/)
### POST request
Sometimes you need more complex things. Like username and password or even files! These things can not be passed by URL. So we use POST method instead.
```python
from django.views.decorators.csrf import csrf_exempt
from django.http import HttpResponse
from .models import Task

@csrf_exempt
def list_create_tasks(request):
	if request.method == 'POST':
		taskText = request.POST.get('task')
		task = Task(task=taskText)
		task.save()
		return HttpResponse(f"Task Created: '{taskText}'")
```
#### Postman
-   **URL**
-   **Body:** form-data
-   **key:** value
### Object-Oriented View
#### Function Based View(FBV)
```python
# views.py
from django.http import HttpResponse

def my_view(request):
    if request.method == 'GET':
        # <view logic>
        return HttpResponse('result')
```
```python
# urls.py
from django.urls import path
from myapp.views import my_view

urlpatterns = [
    path('about/', my_view),
]
```
#### Class Based View(CBV)
```python
# views.py
from django.http import HttpResponse
from django.views import View

class MyView(View):
    def get(self, request):
        # <view logic>
        return HttpResponse('result')
```

```python
# urls.py
from django.urls import path
from myapp.views import MyView

urlpatterns = [
    path('about/', MyView.as_view()),
]
```
### Generic Views
#### ListView
```python
from django.views.generic import ListView
from app.models import User

class UserListView(ListView):
    model = User
    template_name = 'user/list.html'
```
Users objects will be stored in object_list and can be used in template:
```html
{% for user in object_list %}
    {{ user.name }}
{% endfor %}
```
If you don't specify template name, it will look for model_list.html in templates.
##### Pagination
```python
from django.views.generic import ListView
from app.models import User

class UserListView(ListView):
    model = User
    template_name = 'user/list.html'
    paginate_by = 5
```
The url will be: /user-list/?page=1
##### Filtering
```python
from django.views.generic import ListView
from app.models import User

class UserListView(ListView):
    queryset = User.objects.filter(username=='sajjad')
    template_name = 'user/list.html'
    paginate_by = 5
```
**More complex filtering**
```python
from django.views.generic import ListView
from app.models import User

class UserListView(ListView):
    template_name = 'user/list.html'
    paginate_by = 5

    def get_queryset(self):
        return User.objects.filter(username=='sajjad')
```
#### DetailView
```python
# urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('<int:pk>/', views.BookDetailView.as_view()),
]
```
```python
# views.py
from django.views.generic import DetailView

class BookDetailView(DetailView):
	model = Book
	template_name = 'books/book-detail.html'
```
Address will be like: [http://localhost:8000/books/1](http://localhost:8000/books/1)
##### Filtering
```python
from django.views.generic import DetailView

class BookDetailView(DetailView):
    queryset = Book.objects.filter(is_published=True)
```
#### CreateView
```python
from django.views.generic import CreateView
from myapp.forms import BookCreateForm

class BookCreateView(CreateView):
    template_name = 'books/book-create.html'
    form_class = BookCreateForm
```
##### Initial values
```python
# views.py
from django.views.generic import CreateView
from myapp.forms import BookCreateForm

class BookCreateView(CreateView):
    template_name = 'books/book-create.html'
    form_class = BookCreateForm

    def get_initial(self, *args, **kwargs):
        initial = super(BookCreateView, self).get_initial(**kwargs)
        initial['title'] = 'My Title'
        return initial
```
#### UpdateView
```python
from django.views.generic import UpdateView
from myapp.forms import BookCreateForm

class BookUpdateView(UpdateView):
    template_name = 'books/book-update.html'
    form_class = BookUpdateForm
```
#### FormView
```python
from django.views.generic import FormView
from myapp.forms import ContactForm

class ContactView(FormView):
    template_name = 'contact.html'
    form_class = ContactForm
    success_url = '/thanks/'

    def form_valid(self, form):
        # This method is called when valid form data has been POSTed.
        # It should return an HttpResponse.
        form.send_email()
        return super().form_valid(form)
```
#### DeleteView
```python
from django.urls import reverse_lazy
from django.views.generic import DeleteView
from myapp.models import User

class AuthorCreate(DeleteView):
    model = User
	template_name = 'delete.html'
    success_url = reverse_lazy('author-list')
```
`reverse()` used in FBV and `reverse_lazy()` used in CBV.
```html
<form method="post">{% csrf_token %}
    <p>Are you sure you want to delete "{{ object }}"?</p>
    <input type="submit" value="Confirm">
</form>
```
If you don't specify template, it would be model_confirm_delete.html
### Mixin Class
You can use mixin classes everywhere. Like in models:
```python
from django.db import models

class TimestampableMixin(models.Model):
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

class User(TimestampableMixin, models.Model):
	username = models.CharField(max_length=10)
	email = models.EmailField()
	signing_date = models.DateTimeField()
```
Or even in views!
## Templates
Create a folder in your app, called templates. Then you can import html files in it. Now in view, you can use them:
```python
from django.shortcuts import render

def book_detail(request, book_id):
	book = Book.objects.get(pk=book_id)
	context = {'book': book}
	return render(request, 'book_detail.html', context=context)
```
The book_detail.html should look like this(using data):
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Index</title>
</head>
<body>
	<h2>{{ book.author }} wrote {{ book.title }}.</h2>
</body>
</html>
```
You can also write the view using classes:
```python
from django.shortcuts import render
from django.views.generic import TemplateView

class book_detail(TemplateView):
	def get(self, request, book_id):
		book = Book.objects.get(pk=book_id)
		context = {'book': book}
		return render(request, 'book_detail.html', context=context)
```
### Static files(CSS and JS files)
make a folder called static in your project folder and add it in Settings file:
```python
STATICFILES_DIRS = [ os.path.join(BASE_DIR, "static"), '/var/project/static/', ]
```
Now for using it in your templates, add this at the first line:
```html
{% load static %}
```
Then you can use it like {% static file_relative_address %}:
```html
<link rel="stylesheet" type="text/css" href="{% static 'library/home.css' %}">
```
### Variables
dot means different things in templates:
-   If . comes after a dictionary, everything after it, is a key.
-   Can use variables and functions after . to be merged.
-   Can use number after dot to be index of a list.
### Tags
```html
{% tag %} ... tag contents ... {% endtag %}
```
#### Condition
```html
{% if not book.borrowable %}
	This book is not borrowable!
{% elif book.is_borrowed %}
	Someone borrowed this book.
{% else %}
	Yes, it's available.
{% endif %}
```
```html
{% if A and B or C %}
```
You can even use these:
-   ==
-   !=
-   >=
-   <=
-   >
-   <
-   is
-   is not
-   in
-   not in
#### Loops
```html
<ul>
{% for book in book_list %}
    <li>{{ book.title }}</li>
{% endfor %}
</ul>
```
For list with tuple values:
```html
{% for x, y, z in list %}
```
For dictionaries:
```html
{% for key, value in dictionary %}
```
Loop in template makes some variables we can use:
-   **forloop.counter:** place where loop is.
-   **forloop.revcounter:** place where loop is from end.
-   **forloop.first:** will be True if loop is at first.
-   **forloop.last:** will be True if loop is at last.
#### Inheritance
**Base file**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>{% block title %}Home{% endblock %}</title>
    <link rel="stylesheet" href="base.css">
</head>

<body>
    <div id="navigation">
        {% block navigation %}
        <ul>
            <li><a href="/">Home</a></li>
            <li><a href="/">Contact</a></li>
            <li><a href="/">About us</a></li>
        </ul>
        {% endblock %}
    </div>

    <div id="content">
        {% block content %}{% endblock %}
    </div>
</body>
</html>
```
**Child file**
```html
{% extends "base.html" %}

{% block title %}Book List{% endblock %}

{% block content %}
{% for book in book_list %}
    <h2>{{ book.title }}</h2>
    <h3>{{ book.author }}</h3>
{% endfor %}
{% endblock %}
```
### Filters
#### lower
make lowercase variable
```html
{{ value|lower }}
```
#### length
get length of string or list
```html
{{ value|length }}
```
#### filesizeformat
show file size in MB
```html
{{ value|filesizeformat }}
```
#### truncatedwords
make a text truncated to specified number
```html
{{ value|truncatewords:2 }}
```
#### default
if variable is False or None default will be replaced
```html
{{ value|default:"empty" }}
```
#### chained filters
```html
{{ value|lower|truncatewords:2 }}
```
More filters on:
[https://docs.djangoproject.com/en/3.0/ref/templates/builtins/#ref-templates-builtins-filters](https://docs.djangoproject.com/en/3.0/ref/templates/builtins/#ref-templates-builtins-filters)
## Forms
Create form.py in your app:
```python
from django import forms

class SignUpForm(forms.Form):
    username = forms.CharField()
    password = forms.CharField()
    email = forms.EmailField(required=False)
```
### Validation
With .is_valid() function in view, we can validate a form. The output is either True or False.
#### errors
Using this method, a dictionary called cleaned_data will be made with the fields key. You can get errors of each part using form.errors['email'] or something.
#### custom validation
For making custom validation for a field, you need to make a function with the name starting with clean_ and ending with the field's name.
**Example**
```python
from django import forms

class SignUpForm(forms.Form):
    username = forms.CharField()
    password = forms.CharField()
    email = forms.EmailField(required=False)

    def clean_username(self):
        username = self.cleaned_data['username']
        if username.startswith('a'):
            message = "The first letter of username couldn't start with a"
            raise forms.ValidationError(message)
        return username
```
### Form in view
```python
from django.shortcuts import render

def add_book(request):
    if request.method() == "GET":
        form = BookForm()
        return render(request, 'example.html', {'form': form})
```
#### Turning form to object
```python
def add_book(request):
    if request.method == "GET":
        form = BookForm()
        return render(request, 'example.html', {'form': form})
    if requst.method == "POST":
        form = BookForm(request.POST)
        if form.is_valid():
            book = Book()
            book.name = form.cleaned_data['name']
            book.writer = form.cleaned_data['writer']
            book.save()
            return render(request, 'done.html', {})
        return render(request, 'error.html', {})
```
We can also do this without form but it's not an ideal way!
```python
def add_book(request):
    if request.method == "GET":
        form = BookForm()
        return render(request, 'example.html', {'form': form})
    if requst.method == "POST":
        book = Book()
        name = request.POST.get('name')
        writer = request.POST.get('writer')
        try:
            name = clean_name(name)
        except InvalidName:
            return render(request, 'error.html', {})
        try:
            writer = clean_writer(writer)
        except InvalidWriter:
            return render(request, 'error.html', {})

        book.name = name
        book.writer = writer
        book.save()
        return render(request, 'done.html', {})
```
### Form in template
```html
<h2>New User</h2>
<form method="POST" class="post-form">{% csrf_token %}
	{{ form.as_p }}
	<button type="submit" class="save btn btn-default">Save</button>
</form>
```
### ModelForm
ModelForms are simpler ways to write form and use it in views.
Previously saw we wrote:
```python
from django import forms

class SignUpForm(forms.Form):
    username = forms.CharField(max_length=10)
    email = forms.EmailField()
```
Now we write:
```python
from django import forms
from .models import User

class SignUpForm(forms.ModelForm):

    class Meta:
        model = User
        fields = ('username', 'email',)
```
And when we want to use it in views, the form gives us object; Thus we don't need cleaned_data:
```python
def sign_up(request):
    if request.method == "GET":
        form = SignUpForm()
        return render(request, 'example.html', {'form': form})
    if requst.method == "POST":
        form = SignUpForm(request.POST)
        if form.is_valid():
            book = form.save()
```
## Admin
Add models inside your app in admin.py
```python
from django.contrib import admin
from .models import Author, Book

admin.site.register(Book)
admin.site.register(Author)
```
Visit: [localhost:8000/admin](localhost:8000/admin)
### Object representation
To represent, use this in your models:
```python
def __str__(self):
    pass
```
**Example**
```python
from django.db import models

class Author(models.Model):
    first_name = models.CharField(max_length=100)
    last_name = models.CharField(max_length=100)
    birth_date = models.DateField(blank=True, null=True)
    country = models.CharField(max_length=100)

    def __str__(self):
        return f"{self.first_name} {self.last_name}"

class Book(models.Model):
    title = models.CharField(max_length=200)
    publish_date = models.DateField()
    pages_count = models.IntegerField()
    author = models.ForeignKey(
        Author, related_name='books', on_delete=models.CASCADE
    )

    def __str__(self):
        return self.title
```
### Admin Personalization
#### list_display
```python
from django.contrib import admin
from .models import Author, Book

@admin.register(Author)
class AuthorAdmin(admin.ModelAdmin):
    list_display = ['id', 'first_name', 'last_name', 'birth_date', 'country']

@admin.register(Book)
class BookAdmin(admin.ModelAdmin):
    list_display = ['id', 'title', 'publish_date', 'pages_count']
```
#### list_display_links
```python
from django.contrib import admin
from .models import Author, Book

@admin.register(Author)
class AuthorAdmin(admin.ModelAdmin):
    list_display = ['id', 'first_name', 'last_name', 'birth_date', 'country']
		list_display_links = ['id', 'first_name']

@admin.register(Book)
class BookAdmin(admin.ModelAdmin):
    list_display = ['id', 'title', 'publish_date', 'pages_count']
```
**Another way:**
```python
from django.contrib import admin
from django.utils.html import mark_safe
from .models import Product 

@admin.register(Product)
class ProductAdmin(admin.ModelAdmin):
    list_filter = ['category', 'company']
    sortable_by = ['price']
    def name_link(self):
        return mark_safe(f'<a href="{self.id}">{self.name}</a>')
    name_link.short_description = 'name'
    list_display = ['id', name_link, 'category', 'company', 'price']
```
#### sortable_by
```python
@admin.register(Author)
class AuthorAdmin(admin.ModelAdmin):
    # previous codes
    # ....
    sortable_by = ['first_name', 'last_name']
```
#### list_filter
```python
@admin.register(Author)
class AuthorAdmin(admin.ModelAdmin):
    # previous codes
    # ...
    list_filter = ['country']
```
#### list_editable
```python
@admin.register(Author)
class AuthorAdmin(admin.ModelAdmin):
    # previous codes
    # ...
    list_editable = ['country']
```
#### search_fields
```python
@admin.register(Author)
class AuthorAdmin(admin.ModelAdmin):
    # previous codes
    # ...
    search_fields = ['first_name']
```
#### inlines
**StackedInline**
```python
class BookInline(admin.StackedInline):
    model = Book

@admin.register(Author)
class AuthorAdmin(admin.ModelAdmin):
    # previous codes
    # ...
    inlines = [BookInline]
```
**TabularInline**
```python
class ProductTabularInline(admin.TabularInline):
    model = Product

@admin.register(Company)
class CompanyAdmin(admin.ModelAdmin):
    inlines = [ProductTabularInline]
```
#### fields
```python
@admin.register(Author)
class AuthorAdmin(admin.Model):
    # previous codes
    # ...
    fields = [('first_name', 'last_name'), 'country']
```
#### readonly_fields
```python
@admin.register(Author)
class AuthorAdmin(admin.Model):
    # previous codes
    # ...
    readonly_fields = ['country']
```
#### extra
Delete empty fields
```python
from django.contrib import admin
from .models import Company

class ProductTabularInline(admin.TabularInline):
    model = Product
    extra = 0

@admin.register(Company)
class CompanyAdmin(admin.ModelAdmin):
    inlines = [ProductTabularInline]
```
#### fieldsets
```python
@admin.register(Book)
class BookAdmin(admin.ModelAdmin):
    fieldsets = (
        ('General Info', {
            'fields': ('title', 'publish_date')
        }),
        ('Details', {
            'classes': ('collapse',),
            'fields': ('pages_count', 'author', 'price')
        }),
    )
```
#### custom actions
```python
from django.contrib import admin, messages
from django.db.models import F
from .models import Book, Author

@admin.register(Book)
class BookAdmin(admin.ModelAdmin):
    # previous codes
    # ...

    actions = ['add_pages']

    def add_pages(self, request, queryset):
        updated = queryset.update(pages_count=F('pages_count') + 2)
        self.message_user(
            request, f"{updated} books pages added with two", messages.SUCCESS
        )

    add_pages.short_description = 'Add two page to selected books'
```
# Django Rest Framework(DRF)
## Installation
```bash
>>> pip install djangorestframework
```
add 'rest_framework' to INSTALLED_APPS:
```python
INSTALLED_APPS = [
    # Previous apps 
    # ...
    'rest_framework',
]
```
## Views
### Function Based View(FBV)
```python
from rest_framework.decorators import api_view
from rest_framework.response import Response

@api_view(['GET', 'POST'])
def hello(request):

    if request.method == 'GET':
        return Response({"message": "Hello GET!!!"})
    return Response({"message": "Hello POST!!!"})
```
### Class Based View(CBV)
```python
from rest_framework.views import APIView
from rest_framework.response import Response

class HelloAPIView(APIView):
    def get(self, request):
        return Response({"message": "Hello GET!!!"})

    def post(self, request):
        return Response({"message": "Hello POST!!!"})
```
And if you want to authenticate user:

```python
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework.permissions import IsAuthenticated

class ByeAPIView(APIView):
    permission_classes = [IsAuthenticated]

    def post(self, request):
        return Response({"message": "Bye!!!"})
```
**Other views**
-   CreateAPIView
-   UpdateAPIView
-   DestroyAPIView
-   RetrievAPIView
-   GenericAPIView
For more detail check this:
[http://www.cdrf.co/](http://www.cdrf.co/)
## Serializers
### Serializer Class
It's like Form but for api:
```python
from rest_framework import serializers
from .models import Car


class CarSerializer(serializers.Serializer):
    name = serializers.CharField(max_length=100)
    minimum_price = serializers.IntegerField()
    maximum_price = serializers.IntegerField()
    country = serializers.CharField(max_length=100)

    def validate(self, data):
        if data.get('minimum_price', 0) > data.get('maximum_price', 0):
            error = 'Maximum should be greater than minimum'
            raise serializers.ValidationError(error)

        return data

    def create(self, validated_data):
        car = Car.objects.create(**validated_data)
        return car

    def update(self, instance, validated_data):
        instance.name = validated_data.get(
            'name', instance.name
        )
        instance.minimum_price = validated_data.get(
            'minimum_price', instance.minimum_price
        )
        instance.maximum_price = validated_data.get(
            'maximum_price', instance.maximum_price
        )
        instance.country = validated_data.get(
            'country', instance.country
        )

        instance.save()
        return instance  
```
You can also validate each object individually:
```python
# serilizers.py
from rest_framework import serializers
from .models import Car


class CarSerializer(serializers.Serializer):    
    # previous codes
    # ...
    def validate_minimum_price(self ,value):
        if value < 0 :
            raise serializers.ValidationError('Minimum price must be postive')

        return value 
```
```python
# views.py
from rest_framework.response import Response
from rest_framework.views import APIView
from rest_framework.generics import get_object_or_404

from .serializers import CarSerializer
from .models import Car


class AddCarAPIView(APIView):
    def post(self, request):
        car_serializer = CarSerializer(data=request.data)
        if car_serializer.is_valid():
            car_serializer.save()
            return Response({'message': 'Car added successfully!'})

        return Response({'message': car_serializer.errors})


class CarAPIView(APIView):
    def get(self, request, car_id):
        car = get_object_or_404(Car, id=car_id)
        car_serializer = CarSerializer(instance=car)
        data = car_serializer.data
        return Response({'car': data})

    def put(self, request, car_id):
        car = get_object_or_404(Car, id=car_id)
        car_serializer = CarSerializer(
            instance=car, 
            data=request.data, 
            partial=True
        )

        if car_serializer.is_valid():
            car_serializer.save()
            return Response({'message': 'Car updated successfully!'})

        return Response({'message': car_serializer.errors})
```
### ModelSerializer Class
It's like ModelForm but for api:
```python
# serializers.py
from rest_framework import serializers
from .models import Car


class CarSerializer(serializers.ModelSerializer):
    class Meta:
        model = Car
        fields = ('name', 'minimum_price', 'maximum_price', 'country')

    def validate(self, data):
        if data.get('minimum_price', 0) > data.get('maximum_price', 0):
            error = 'Maximum should be greater than minimum'
            raise serializers.ValidationError(error)

        return data

    def validate_minimum_price(self ,value):
        if value < 0 :
            raise serializers.ValidationError('Minimum price must be postive')

        return value
```
## Authentication with Token
Add this to your settings.py:
```python
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'rest_framework.authentication.TokenAuthentication',
    ],
}
```
And add this to INSTALLED_APPS:
```python
INSTALLED_APPS = [
    # Other apps
    'rest_framework.authtoken',
]
```
Now migrate your project:
```
python manage.py migrate
```
### Login
Now in urls.py use this(default drf view):
```python
from django.urls import path
from rest_framework.authtoken.views import obtain_auth_token

urlpatterns = [
    # Other urls
    path('login/', view=obtain_auth_token),
]
```
Now if you request with username and password, you get token.
Now you can add limitations to a view:
```python
from rest_framework.response import Response
from rest_framework.generics import GenericAPIView
from rest_framework.permissions import IsAuthenticated


class HelloAPIView(GenericAPIView):
    permission_classes = (IsAuthenticated, )

    def get(self, request):
        return Response(data={'message': f"Hello {request.user.username}!"})
```
Now if you don't prepare token, You can't access the view(token in header).
### Logout
```python
class LogoutAPIView(GenericAPIView):

    permission_classes = (IsAuthenticated, )

    def post(self, request):
        request.user.auth_token.delete()
        return Response(data={'message': f"Bye {request.user.username}!"})
```
## Permissions
### Famouse built-in permission
**IsAdminUser**
**IsAuthenticated**
**IsAuthenticatedOrReadOnly**
**AllowAny**
**DjangoModelPermissions**
### Types of permissions
#### has_permission
```python
from rest_framework.permissions import BasePermission
from security.models import Blacklist


class Blacklisted(BasePermission):

    def has_permission(self, request, view):
        ip_address = request.META['REMOTE_ADDR']
        in_blacklist = Blacklist.objects.filter(ip=ip_address).exists()

        return not in_blacklist
```
#### has_object_permission
```python
from rest_framework.permissions import BasePermission, SAFE_METHODS

# SAFE_METHODS = ['GET', 'HEAD', 'OPTIONS']

class IsAuthorOrReadOnly(BasePermission):

    def has_object_permission(self, request, view, obj):

        if request.method in SAFE_METHODS:
            return True

        return obj.author == request.user  
```
DRF checks has_permission automatically, but for checking has_object_permission, we should call it:
```python
from rest_framework.generics import GenericAPIView, get_object_or_404
from rest_framework.permissions import IsAuthenticated
from rest_framework.response import Response

from .models import Article
from .permissions import IsAuthorOrReadOnly, Blacklisted
from .serializers import ArticleSerializer


class ArticleAPIView(GenericAPIView):

    permission_classes = (IsAuthenticated, IsAuthorOrReadOnly, Blacklisted)
    serializer_class = ArticleSerializer

    def get(self, request, article_id):
        article = get_object_or_404(Article, id=article_id)

        data = self.get_serializer(article).data

        return Response(data={'article': data})

    def put(self, request, article_id):
        article = get_object_or_404(Article, id=article_id)
        self.check_object_permissions(request, article)

        # Do some changes on article
        # ...

        return Response(data={'message': 'Updated!'))
```
`self.check_object_permissions(request, article)` is calling that!
## CORS(Cross-Origin Resource Sharing)
Origin includes protocol(http, https, ...), host(localhost, ...) and a port number(8000, ...). Cross-Origin means a request outside of an origin.
### corsheaders
```
pip install django-cors-headers
```
Add it in INSTALLED_APPS:
```python
INSTALLED_APPS = [
    ...
    'corsheaders',
    ...
]
```
Add the CorsMiddleware before CommonMiddleware:
```python
MIDDLEWARE = [
    ...
    'corsheaders.middleware.CorsMiddleware',
    'django.middleware.common.CommonMiddleware',
    ...
]
```
Now you can customize CORS allowed origins.
**To allow all requests:**
```python
CORS_ALLOW_ALL_ORIGINS = True
```
**To allow custom requests:**
```python
CORS_ALLOWED_ORIGINS = [
    "https://frontend-app.ir",
    "http://localhost:8000",
    "http://127.0.0.1:8000",
]
```
# Testing
Create a tests folder in your app and create your test files in it(They should start with test).
Django uses python unit testing. So you can read them in [https://docs.python.org/3/library/unittest.html#assert-methods](https://docs.python.org/3/library/unittest.html#assert-methods).
Also read [https://docs.djangoproject.com/en/dev/topics/testing/tools/#assertions](https://docs.djangoproject.com/en/dev/topics/testing/tools/#assertions).
## Model Testing
Assume we have this model:
```python
from django.db import models
import datetime

class Person(models.Model):    
    first_name = models.CharField(max_length=100)
    last_name = models.CharField(max_length=100)
    birth_date = models.DateField()

    def is_young(self):        
        return self.birth_date > datetime.date(1990, 1, 1)

    def is_old(self):
        return self.birth_date < datetime.date(1960, 1, 1)

    def __str__(self):
        return self.first_name
```
We can test it:
```python
from django.test import TestCase
from app.models import Person
import datetime

class PersonTests(TestCase):
    def setUp(self):
        Person.objects.create(first_name='Ali', last_name='Taghipour', birth_date=datetime.date(2000, 4, 12))
        Person.objects.create(first_name='Maryam', last_name='Noori', birth_date=datetime.date(1956, 11, 10))    

    def test_is_young(self):        
        ali = Person.objects.get(first_name='Ali')        
        maryam = Person.objects.get(first_name='Maryam')        
        self.assertTrue(ali.is_young())        
        self.assertFalse(maryam.is_young())    

    def test_is_old(self):        
        ali = Person.objects.get(first_name='Ali')        
        maryam = Person.objects.get(first_name='Maryam')        
        self.assertFalse(ali.is_old())        
        self.assertTrue(maryam.is_old())

    def test_str(self):
        ali = Person.objects.get(first_name='Ali')
        maryam = Person.objects.get(first_name='Maryam')
        self.assertEqual(ali.__str__(), 'Ali')
        self.assertEqual(maryam.__str__(), 'Maryam')
```
## View Testing
```python
from django.test import TestCase


class ViewTests(TestCase):
    def test_login(self):
        data = {'username': 'ali', 'password': '1375'}
        response = self.client.post('/login/', data=data)
        self.assertEqual(response.status_code, 200)

    def test_homepage(self):
        response = self.client.get('/homepage/')
        self.assertTrue(b'Welcome to the Homepage' in response.content)
```
```python
from django.test import TestCase


class ViewTests(TestCase):
    def test_string_welcome(self):
        response = self.client.get('/string_welcome/')
        string_data = response.content.decode('utf-8')
        self.assertEqual('welcome to quera', string_data)
```
### Testing API
```python
from django.test import TestCase


class ViewTests(TestCase):
    def test_welcome(self):
        response = self.client.get('/welcome/')
        data = response.json()
        self.assertEqual(data.get('message'), 'welcome to quera')
```
## Test Coverage
### Installation
```
pip install coverage
```
### Run the Coverage
```
coverage run --source='.' manage.py test app_name
```
### Result of Coverage
#### Terminal
```
coverage report
```
#### HTML
```
coverage html
```
## Fixtures
First create a folder called fixtures in your app. Then make a JSON, XML or YAML for the data:
```json
[
	{
		"model": "library_management.person",
		"pk": 1,
		"fields": {
			"first_name": "Abbas",
			"last_name": "Azizi",
			"birth_date": "1996-06-13"
		}
	},
	{
		"model": "library_management.person",
		"pk": 2,
		"fields": {
			"first_name": "Nazanin",
			"last_name": "Aliabadi",
			"birth_date": "1950-02-05"
		}
	}
]
```
for reffering to the model, you should use app_name and the model name.
PK stands for primary key. It used for reffering to forieng keys and stuff.
Now you can import your fixture:
```python
class PersonTests(TestCase):    

    fixtures = ['person.json']    

    def setUp(self):
        Person.objects.create(first_name='Ali', last_name='Taghipour', birth_date=datetime.date(2000, 4, 12))
        Person.objects.create(first_name='Maryam', last_name='Noori', birth_date=datetime.date(1956, 11, 10))    

    def test_is_young(self):        
        ali = Person.objects.get(first_name='Abbas')        
        maryam = Person.objects.get(first_name='Maryam')        
        self.assertTrue(ali.is_young())        
        self.assertFalse(maryam.is_young())    

    def test_is_old(self):        
        ali = Person.objects.get(first_name='Ali')        
        maryam = Person.objects.get(first_name='Nazanin')        
        self.assertFalse(ali.is_old())        
        self.assertTrue(maryam.is_old())
```
You can also use fixtures to import in your main database:
```bash
python manage.py loaddata "Fixture Name"
```
Also you can make a fixture with your present database:
```bash
python manage.py dumpdata > "Fixture Name"
```
You can also make a fixture with parts of your django app:
```bash
python manage.py dumpdata "Table Name" > "Fixture Name"
```
```bash
python manage.py dumpdata "Django App" > "Fixture Name"
```
## Django Debug Toolbar
### Installation
```shell
pip install django-debug-toolbar
```
Add this to your INSTALLED_APPS:
```python
INSTALLED_APPS = [
    # ...
    'debug_toolbar',
]
```
Add this to your main urls.py:
```python
import debug_toolbar
from django.urls import include, path
# Other Imports

urlpatterns = [
    # ...
    path('__debug__/', include(debug_toolbar.urls)),
]
```
Add this to your first line MIDDLEWARE:
```python
MIDDLEWARE = [
    'debug_toolbar.middleware.DebugToolbarMiddleware',
    # ...
]
```
Add this at the end of the settings.py:
```python
INTERNAL_IPS = [
    '127.0.0.1',
]
```
