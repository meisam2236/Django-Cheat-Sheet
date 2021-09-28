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
