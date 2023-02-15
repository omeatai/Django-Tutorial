# Django-Tutorial

Django Tutorial by Ifeanyi Omeata

## Tutorial

---

### [1-DJANGO & DJANGO REST FRAMEWORK WITH REACT FRONTEND - PARWIZ FOROGH](https://www.udemy.com/course/django-django-rest-framework-build-rest-api-in-python/learn/lecture/29085424#overview)

+BUILDING BLOG APPLICATION IN DJANGO

<details>
  <summary>1. Introduction </summary>

Install virtual env:

```py
python -m venv venv_dj_blog
```

Activate virtual env:

```py
source venv_dj_blog/bin/activate
```

Install Django:

```py
python -m pip install Django
pip install Django
pip install Django==4.1.5
```

Upgrade pip:

```py
python -m pip install --upgrade pip
```

Create Requirements:

```py
pip freeze > requirements.txt
```

Create Project:

```py
django-admin startproject Blog
django-admin startproject Blog .
```

Run Migrations:

```py
#python manage.py makemigrations
python manage.py migrate
```

Start local Server:

```py
python manage.py runserver
```

</details>

<details>
  <summary>2. Create Super User</summary>

```py
python manage.py createsuperuser
```

```py
# Username (leave blank to use 'ifeanyiomeata'): admin
# Email address: admin@gmail.com
# Password:
# Password (again):
# This password is too short. It must contain at least 8 characters.
# This password is too common.
# This password is entirely numeric.
# Bypass password validation and create user anyway? [y/N]: y
# Superuser created successfully.
# (venv-djblog) ifeanyiomeata@Ifeanyis-MacBook-Air djblog %
```

```py
python manage.py runserver
```

http://127.0.0.1:8000/admin/:

![](https://user-images.githubusercontent.com/32337103/215768567-5d80987f-c627-444a-b384-9591a942f9fb.png)

![](https://user-images.githubusercontent.com/32337103/215768464-b29c8b45-2baa-4271-8bdb-17478160e42e.png)

</details>

<details>
  <summary>3. Create App - articles </summary>

```py
python manage.py startapp articles
```

Register App in Settings.py:

```py

# Application definition

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'articles'
]

```

</details>

<details>
  <summary>4. Create Function View  </summary>

articles/views.py:

```py
from django.shortcuts import render, HttpResponse

# Create your views here.

def article_list(request):
    return HttpResponse("This is our first view for articles")
```

articles/urls.py:

```py
from django.urls import path
from .views import article_list

urlpatterns = [
    path('', article_list, name='article_list')
]

```

djblog/urls.py:

```py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('articles.urls') )
]

```

![](https://user-images.githubusercontent.com/32337103/215846696-05e3e185-0a45-40d4-a0ef-ea06625a9169.png)

</details>

<details>
  <summary>5. Create Article model</summary>

articles/models.py:

```py
from django.db import models
from django.contrib.auth.models import User

# Create your models here.
class Article(models.Model):
    title = models.CharField(max_length=200)
    description = models.TextField()
    slug = models.SlugField(max_length=100, unique=True)
    published = models.DateTimeField (auto_now_add=True)
    author = models.ForeignKey(User, on_delete=models.CASCADE)
```

Run Migrations:

```py
python manage.py makemigrations
python manage.py migrate
```

</details>

<details>
  <summary>6. Types of Model Relationships</summary>

Many to One Relationship:

```py
class Category(models.Model):
  name = models.CharField(max_length=30)

class Article(models.Model):
  title = models.CharField(max_length=30)
  description = models.CharField(max_length=100)
  category = models.ForeignKey(Category, on_delete=models.CASCADE)
```

One to One Relationship:

```py
class Place(models.Model):
  name = models.CharField(max_length=50)
  address = models.CharField(max_length=80)

class Restaurant(models.Model):
  place = models.OneToOneField(Place, on_delete=models.CASCADE, primary_key=True)
  serves_hot_dogs = models.BooleanField(default=False)
  serves_pizza = models.BooleanField(default=False)
```

Many to Many Relationship:

```py
class Publication(models.Model):
  title = models.CharField(max_length=30)

class Article(models.Model):
  headline = models.CharField(max_length=100)
  publications = models.ManyToManyField(Publication)
```

</details>

<details>
  <summary>7. Register Article Model in Admin</summary>

![](https://user-images.githubusercontent.com/32337103/215871020-7c6ebe88-6df2-4fa3-8d8a-f378481b3315.png)

article/admin.py:

```py
from django.contrib import admin
from .models import Article

# Register your models here.
admin.site.register(Article)
```

![](https://user-images.githubusercontent.com/32337103/215872036-54c00bc1-9ecb-46e4-86af-2fd266b1fc00.png)

![](https://user-images.githubusercontent.com/32337103/215872428-284492e9-a096-486e-a602-d52469d37f7a.png)

![](https://user-images.githubusercontent.com/32337103/215872531-f492e5bb-65e3-43e0-8e56-f0d026e9e381.png)

article/models.py:

```py
from django.db import models
from django.contrib.auth.models import User

# Create your models here.
class Article(models.Model):
    title = models.CharField(max_length=200)
    description = models.TextField()
    slug = models.SlugField(max_length=100, unique=True)
    published = models.DateTimeField (auto_now_add=True)
    author = models.ForeignKey(User, on_delete=models.CASCADE)

    def __str__(self) -> str:
        return self.title
```

![](https://user-images.githubusercontent.com/32337103/215873268-146b239e-df87-4444-858f-8b032f0e38a6.png)

article/admin.py:

```py
from django.contrib import admin
from .models import Article

# Register your models here.
#admin.site.register(Article)

@admin.register(Article)
class ArticleAdmin(admin.ModelAdmin):
    list_display = ('title', 'published', 'author')
    date_hierarchy = 'published'
    search_fields = ('title', 'description')
```

![](https://user-images.githubusercontent.com/32337103/215874163-e718c175-a845-437c-9e79-fb07cc9a3ee1.png)

</details>

<details>
  <summary>8. Create Django Templates </summary>

Required arguments:

- request: The request object is used to generate the response.
- template_name: The full name of a template to use or sequence of template names.

Optional Arguments:

- context: A dictionary of values to add to the template context. By default, this is an
  empty dictionary. If a value in the dictionary is callable, the view will call it just before rendering the template.
- content_type: The MIME type to use for the resulting document. Defaults to 'text/html".
- status: The status code for the response. Defaults to 200.

There are two types of Templates:

- App labeled templates
- Project labeled templates

App labeled templates -

articles/templates/articles.html:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Article List</title>
  </head>
  <body>
    <h1>Article List</h1>
    <p>This is the place to render articles from Database.</p>
  </body>
</html>
```

articles/views.py:

```py
from django.shortcuts import render, HttpResponse

# Create your views here.

def article_list(request):
    # return HttpResponse("This is our first view for articles")
    return render(request, 'articles.html')

```

![](https://user-images.githubusercontent.com/32337103/215956850-66d70521-554a-4453-9ba1-8ede5bb0b7c6.png)

</details>

<details>
  <summary>9. Render Context in Template </summary>

articles/views.py:

```py
from django.shortcuts import render, HttpResponse

# Create your views here.

def article_list(request):
    article = "This is my first article title"
    return render(request, 'articles.html', {'article':article})

```

articles/templates/articles.html:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Article List</title>
  </head>
  <body>
    <h1>Article List</h1>
    <p>This is the place to render articles from Database.</p>
    <p>{{article}}</p>
  </body>
</html>
```

![](https://user-images.githubusercontent.com/32337103/215957683-f601ae19-f429-40bb-a551-b2a0f516294c.png)

</details>

<details>
  <summary>10. Project Labeled Templates</summary>

articles/views.py:

```py
from django.shortcuts import render, HttpResponse

# Create your views here.

def article_list(request):
    article = "This is my first article title"
    return render(request, 'articles.html', {'article':article})

```

djblog/templates/articles.html:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Article List</title>
  </head>
  <body>
    <h1>Article List</h1>
    <p>This is the place to render articles from Database.</p>
    <p>{{article}}</p>
  </body>
</html>
```

djblog/settings.py:

```py
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': ['templates'], #<--------- Add your own template directory
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```

![](https://user-images.githubusercontent.com/32337103/215957683-f601ae19-f429-40bb-a551-b2a0f516294c.png)

</details>

<details>
  <summary>11. Template Inheritance - Using Layouts </summary>

articles/views.py:

```py
from django.shortcuts import render, HttpResponse

# Create your views here.

def article_list(request):
    article = "This is my first article title"
    return render(request, 'articles.html', {'article':article})

```

articles/templates/base.html:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>{% block title %}{% endblock title %}</title>
    {% block style %}{% endblock style %}
  </head>
  <body>
    {% block body %}{% endblock body %}
  </body>
</html>
```

articles/templates/articles.html:

```py
{% extends 'base.html' %}

{% block title %} Article List {% endblock title%}

{%block body %}
<h1>Article List</h1>
<p>This is the place to render articles from Database.</p>
<p>{{article}}</p>
{% endblock body %}

```

![](https://user-images.githubusercontent.com/32337103/215979562-be0db0b4-0836-44bf-b048-91f6218e09b2.png)

</details>

<details>
  <summary>12. Adding CSS and Bootstrap without Static</summary>

articles/templates/base.html:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>{% block title %}{% endblock title %}</title>
    {% block style %}{% endblock style %}
  </head>
  <body>
    {% block body %}{% endblock body %}
  </body>
</html>
```

articles/templates/articles.html:

```py
{% extends 'base.html' %}

{% block title %} Article List {% endblock title%}

{% block style %}
<style>
    .h1-style {
        color: red;
    }
</style>
{% endblock style %}

{%block body %}
<h1 class="h1-style">Article List</h1>
<p>This is the place to render articles from Database.</p>
<p>{{article}}</p>
{% endblock body %}

```

![](https://user-images.githubusercontent.com/32337103/215998482-7e12958b-e985-4576-af4d-6ba1ddb29200.png)

</details>

<details>
  <summary>13. Adding Bootstrap Styles </summary>

```bs
https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/css/bootstrap.min.css

<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/css/bootstrap.min.css" integrity="sha384-rbsA2VBKQhggwzxH7pPCaAqO46MgnOM80zW1RWuH61DGLwZJEdK2Kadq2F9CUG65" crossorigin="anonymous">

<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-GLhlTQ8iRABdZLl6O3oVMWSktQOp6b7In1Zl3/Jr59b6EGGoI1aFkw7cmDA6j6gD" crossorigin="anonymous">
```

```bs
https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/js/bootstrap.min.js

<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/js/bootstrap.min.js" integrity="sha384-cuYeSxntonz0PPNlHhBs68uyIAVpIIOZZ5JqeqvYYIcEL727kskC66kF92t6Xl2V" crossorigin="anonymous"></script>

<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/js/bootstrap.bundle.min.js" integrity="sha384-w76AqPfDkMBDXo30jS1Sgez6pr3x5MlQ1ZAGC+nuZB+EYdgRZgiwxhTBTkF7CXvN" crossorigin="anonymous"></script>
```

articles/templates/base.html:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>{% block title %}{% endblock title %}</title>
    <link
      href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/css/bootstrap.min.css"
      rel="stylesheet"
      integrity="sha384-GLhlTQ8iRABdZLl6O3oVMWSktQOp6b7In1Zl3/Jr59b6EGGoI1aFkw7cmDA6j6gD"
      crossorigin="anonymous"
    />
    {% block style %}{% endblock style %}
  </head>
  <body>
    {% block body %}{% endblock body %}
    <script
      src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/js/bootstrap.bundle.min.js"
      integrity="sha384-w76AqPfDkMBDXo30jS1Sgez6pr3x5MlQ1ZAGC+nuZB+EYdgRZgiwxhTBTkF7CXvN"
      crossorigin="anonymous"
    ></script>
  </body>
</html>
```

![](https://user-images.githubusercontent.com/32337103/216303473-6288b019-e6d3-4179-b0ad-db6500e4f1e0.png)

</details>

<details>
  <summary>14. What are signals? </summary>

- Django includes a “signal dispatcher” which helps decoupled applications get notified when actions occur elsewhere in the framework.
- In a nutshell, signals allow certain senders to notify a set of receivers that some action has taken place.
- They’re especially useful when many pieces of code may be interested in the same events.
- For example, a third-party app can register to be notified of settings changes.

```py
from django.apps import AppConfig
from django.core.signals import setting_changed

def my_callback(sender, **kwargs):
    print("Setting changed!")

class MyAppConfig(AppConfig):
    ...

    def ready(self):
        setting_changed.connect(my_callback)
```

```py
from django.core.signals import request_finished

request_finished.connect(my_callback)

def my_callback(sender, **kwargs):
    print("Request finished!")
```

```py
Signal.connect(receiver, sender=None, weak=True, dispatch_uid=None)
```

- receiver – The callback function which will be connected to this signal.
- sender – Specifies a particular sender to receive signals from.
- weak – Django stores signal handlers as weak references by default. Thus, if your receiver is a local function, it may be garbage collected. To prevent this, pass weak=False when you call the signal’s connect() method.
- dispatch_uid – A unique identifier for a signal receiver in cases where duplicate signals may be sent.

Model signals -

- All built-in signals are sent using the send() method.
- The django.db.models.signals module defines a set of signals sent by the model system.
- Signals can make your code harder to maintain.
- Consider implementing a helper method on a custom manager, to both update your models and perform additional logic, or else overriding model methods before using model signals.
- Many of these signals are sent by various model methods like \_init\_\_() or save() that you can override in your own code.
- If you override these methods on your model, you must call the parent class’ methods for these signals to be sent.
- Note also that Django stores signal handlers as weak references by default, so if your handler is a local function, it may be garbage collected.
- To prevent this, pass weak=False when you call the signal’s connect().
- For performance reasons, you shouldn’t perform queries in receivers of pre_init or post_init signals because they would be executed for each instance returned during queryset iteration.

pre_init -

```py
django.db.models.signals.pre_init
```

- Whenever you instantiate a Django model, this signal is sent at the beginning of the model’s \_init\_\_() method.

Arguments sent with this signal:

- sender - The model class that just had an instance created.
- args - A list of positional arguments passed to \_init\_\_().
- kwargs - A dictionary of keyword arguments passed to \_init\_\_().

post_init -

```py
django.db.models.signals.post_init
```

- Like pre_init, but this one is sent when the \_init\_\_() method finishes.

Arguments sent with this signal:

- sender - The model class that just had an instance created.
- instance - The actual instance of the model that’s just been created.

pre_save -

```py
django.db.models.signals.pre_save
```

- This is sent at the beginning of a model’s save() method.

Arguments sent with this signal:

- sender - The model class.
- instance - The actual instance being saved.
- raw - A boolean; True if the model is saved exactly as presented (i.e. when loading a fixture). One should not query/modify other records in the database as the database might not be in a consistent state yet.
- using - The database alias being used.
- update_fields - The set of fields to update as passed to Model.save(), or None if update_fields wasn’t passed to save().

post_save -

```py
django.db.models.signals.post_save
```

- Like pre_save, but sent at the end of the save() method.

Arguments sent with this signal:

- sender - The model class.
- instance - The actual instance being saved.
- created - A boolean; True if a new record was created.
- raw - A boolean; True if the model is saved exactly as presented (i.e. when loading a fixture). One should not query/modify other records in the database as the database might not be in a consistent state yet.
- using - The database alias being used.
- update_fields - The set of fields to update as passed to Model.save(), or None if update_fields wasn’t passed to save().

pre_delete -

```py
django.db.models.signals.pre_delete
```

- Sent at the beginning of a model’s delete() method and a queryset’s delete() method.

Arguments sent with this signal:

- sender - The model class.
- instance - The actual instance being deleted.
- using - The database alias being used.

post_delete -

```py
django.db.models.signals.post_delete
```

- Like pre_delete, but sent at the end of a model’s delete() method and a queryset’s delete() method.

Arguments sent with this signal:

- sender - The model class.
- instance - The actual instance being deleted.
  Note that the object will no longer be in the database, so be very careful what you do with this instance.
- using - The database alias being used.

Examples -

django post_save signals

Let’s have a look on the post_save built-in signal. Its code lives in the django.db.models.signals module. This particular signal fires right after a model finish executing its save method.

```py
from django.contrib.auth.models import User
from django.db.models.signals import post_save

def save_profile(sender, instance, **kwargs):
    instance.profile.save()

post_save.connect(save_profile, sender=User)
```

Another way to register a signal, is by using the @receiver decorator:

```py
from django.contrib.auth.models import User
from django.db.models.signals import post_save
from django.dispatch import receiver

@receiver(post_save, sender=User)
def save_profile(sender, instance, **kwargs):
    instance.profile.save()
```

```py
from django.dispatch import receiver

from djoser.signals import user_activated

@receiver(user_activated)
def my_handler(user, request):
    # do what you need here
```

More Explanation of Django signals:

- The Django Signals is a strategy to allow decoupled applications to get notified when certain events occur
- There are two key elements in the signals machinary: the senders and the receivers.
- As the name suggests, the sender is the one responsible to dispatch a signal, and the receiver is the one who will receive this signal and then do something.
- A receiver must be a function or an instance method which is to receive signals.
- A sender must either be a Python object, or None to receive events from any sender.
- The connection between the senders and the receivers is done through “signal dispatchers”, which are instances of Signal, via the connect method.
- So to receive a signal, you need to register a receiver function that gets called when the signal is sent by using the Signal.connect() method.

django built-in signals:

- Django provides a set of built-in signals that let user code get notified by Django itself of certain actions.

These include some useful notifications:

- django.db.models.signals.pre_save & django.db.models.signals.post_save:
  Sent before or after a model's save() method is called.

- django.db.models.signals.pre_delete & django.db.models.signals.post_delete:
  Sent before or after a model's delete() method or queryset's delete() method is called.

- django.db.models.signals.m2m_changed :
  Sent when a ManyToManyField on a model is changed.

- django.core.signals.request_started & django.core.signals.request_finished:
  Sent when Django starts or finishes an HTTP request.

</details>

<details>
  <summary>15. Dynamic Slugs with signals </summary>

articles/signals.py:

```py
from django.db.models.signals import pre_save
from django.dispatch import receiver
from .models import Article
from django.utils.text import slugify


@receiver(pre_save, sender=Article)
def add_slug(sender, instance, *args, **kwargs):
    if instance and not instance.slug:
        slug = slugify (instance.title)
        instance.slug = slug
```

articles/apps.py:

```py
from django.apps import AppConfig


class ArticlesConfig(AppConfig):
    default_auto_field = 'django.db.models.BigAutoField'
    name = 'articles'

    def ready(self):
        import articles.signals

```

articles/init.py:

```py
default_app_config = "articles.apps.ArticlesConfig"
```

articles/models.py:

```py
from django.db import models
from django.contrib.auth.models import User

# Create your models here.
class Article(models.Model):
    title = models.CharField(max_length=200)
    description = models.TextField()
    slug = models.SlugField(max_length=100, unique=True)
    published = models.DateTimeField (auto_now_add=True)
    author = models.ForeignKey(User, on_delete=models.CASCADE)

    def __str__(self) -> str:
        return self.title
```

```py
python manage.py shell
```

```bs
from django.contrib.auth import get_user_model
from articles.models import Article
user = get_user_model()
 u = user.objects.first()
 >>> u
<User: admin>

article = Article(title='the first article',description='this is the article',author=u)
article.save()
```

```bs
>>> Article.objects.all().last()
```

```py
# <Article: the first article>
```

```bs
>>> Article.objects.all().last().__dict__
```

```py
# {'_state': <django.db.models.base.ModelState object at 0x1044d1100>, 'id': 2, 'title': 'the first article', 'description': 'this is the article', 'slug': 'the-first-article', 'published': datetime.datetime(2023, 2, 2, 12, 31, 24, 759019, tzinfo=datetime.timezone.utc), 'author_id': 1}
```

```bs
>>> Article.objects.all().last().slug
```

```py
# 'the-first-article'
```

```bs
>>> Article.objects.get(author=u, title='the first article')
```

```py
# <Article: the first article>
```

![](https://user-images.githubusercontent.com/32337103/216329675-2b6a670c-ed46-4491-b8cd-17571c12f220.png)

![](https://user-images.githubusercontent.com/32337103/216329839-a8c8d2a8-1b71-4c8c-9f7a-b17b7024f80e.png)

</details>

<details>
  <summary>16. Retrieving Data from Model </summary>

articles/views.py:

```py
from django.shortcuts import render, HttpResponse
from .models import Article

# Create your views here.

def article_list(request):
    articles = Article.objects.all().order_by('-published')
    return render(request, 'articles.html', {'articles':articles})

```

articles/templates/base.html:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>{% block title %}{% endblock title %}</title>
    <link
      href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/css/bootstrap.min.css"
      rel="stylesheet"
      integrity="sha384-GLhlTQ8iRABdZLl6O3oVMWSktQOp6b7In1Zl3/Jr59b6EGGoI1aFkw7cmDA6j6gD"
      crossorigin="anonymous"
    />
    {% block style %}{% endblock style %}
  </head>
  <body>
    {% block body %}{% endblock body %}
    <script
      src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/js/bootstrap.bundle.min.js"
      integrity="sha384-w76AqPfDkMBDXo30jS1Sgez6pr3x5MlQ1ZAGC+nuZB+EYdgRZgiwxhTBTkF7CXvN"
      crossorigin="anonymous"
    ></script>
  </body>
</html>
```

articles/templates/articles.html:

```py
{% extends 'base.html' %}

{% block title %} Article List {% endblock title%}

{% block style %}
<style>
    .link-style {
        text-decoration: none;
        color:black;
    }
    .link-style:hover {
        text-decoration: none;
        color:gray;
    }
</style>
{% endblock style %}

{%block body %}
<section class="container">
    <h1>Article List</h1>
    {% for article in articles %}
        <span class="badge rounded-pill text-bg-success">Author: {{article.author}}</span>
        <h1><a class="link-style" href="">{{article.title}}</a></h1>
    {% endfor %}
</section>

{% endblock body %}

```

![](https://user-images.githubusercontent.com/32337103/216396649-ec8013a9-c735-4925-836d-5da5514493e8.png)

</details>

<details>
  <summary>17. Adding Bootstrap Navbar </summary>

articles/templates/navbar.html:

```html
<nav class="navbar navbar-expand-lg" style="background-color: #e3f2fd;">
  <div class="container">
    <a class="navbar-brand" href="#">Django-Blog</a>
    <button
      class="navbar-toggler"
      type="button"
      data-bs-toggle="collapse"
      data-bs-target="#navbarNavDropdown"
      aria-controls="navbarNavDropdown"
      aria-expanded="false"
      aria-label="Toggle navigation"
    >
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarNavDropdown">
      <ul class="navbar-nav">
        <li class="nav-item">
          <a class="nav-link disabled">Welcome, Adam.</a>
        </li>
        <li class="nav-item">
          <a class="nav-link active" aria-current="page" href="#">Home</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="#">Add Article</a>
        </li>
        <li class="nav-item dropdown">
          <a
            class="nav-link dropdown-toggle"
            href="#"
            role="button"
            data-bs-toggle="dropdown"
            aria-expanded="false"
          >
            Profile
          </a>
          <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="#">Change Password</a></li>
            <li><a class="dropdown-item" href="#">Logout</a></li>
          </ul>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="#">Login</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="#">Register</a>
        </li>
      </ul>
    </div>
  </div>
</nav>
```

articles/templates/base.html:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>{% block title %}{% endblock title %}</title>
    <link
      href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/css/bootstrap.min.css"
      rel="stylesheet"
      integrity="sha384-GLhlTQ8iRABdZLl6O3oVMWSktQOp6b7In1Zl3/Jr59b6EGGoI1aFkw7cmDA6j6gD"
      crossorigin="anonymous"
    />
    {% block style %}{% endblock style %}
  </head>
  <body>
    {% block nav %} {% include 'navbar.html' %} {% endblock nav %} {% block body
    %}{% endblock body %}
    <script
      src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/js/bootstrap.bundle.min.js"
      integrity="sha384-w76AqPfDkMBDXo30jS1Sgez6pr3x5MlQ1ZAGC+nuZB+EYdgRZgiwxhTBTkF7CXvN"
      crossorigin="anonymous"
    ></script>
  </body>
</html>
```

articles/templates/articles.html:

```py
{% extends 'base.html' %}

{% block title %} Article List {% endblock title%}

{% block style %}
<style>
    .h1-style {
        margin: 2rem 0;
        color: #2c9df0;
    }
    .link-style {
        text-decoration: none;
        color:black;
    }
    .link-style:hover {
        text-decoration: none;
        color:gray;
    }
</style>
{% endblock style %}

{%block body %}
<section class="container">
    <h1 class="h1-style">Article List</h1>
    {% for article in articles %}
        <span class="badge rounded-pill text-bg-success">Author: {{article.author}}</span>
        <h1><a class="link-style" href="">{{article.title}}</a></h1>
    {% endfor %}
</section>

{% endblock body %}

```

![](https://user-images.githubusercontent.com/32337103/216427207-449b2da7-1cdc-4515-a942-10ed1173046c.png)

</details>

<details>
  <summary>18. Getting the details of Articles </summary>

articles/views.py:

```py
from django.shortcuts import render, HttpResponse, get_object_or_404
from .models import Article

# Create your views here.

def article_list(request):
    articles = Article.objects.all().order_by('-published')
    return render(request, 'articles.html', {'articles':articles})

def article_details(request, slug):
    article = get_object_or_404(Article, slug=slug)
    return render(request, 'details.html', {'article':article})

```

articles/urls.py:

```py
from django.urls import path
from .views import article_list, article_details

urlpatterns = [
    path('articles/', article_list, name='article_list'),
    path('articles/<slug:slug>/', article_details, name='article_details')
]
```

articles/templates/base.html:

```py
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>{% block title %}{% endblock title %}</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-GLhlTQ8iRABdZLl6O3oVMWSktQOp6b7In1Zl3/Jr59b6EGGoI1aFkw7cmDA6j6gD" crossorigin="anonymous">
    {% block style %}{% endblock style %}
  </head>
  <body>
    {% block nav %}
      {% include 'navbar.html' %}
    {% endblock nav %}
    {% block body %}{% endblock body %}
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/js/bootstrap.bundle.min.js" integrity="sha384-w76AqPfDkMBDXo30jS1Sgez6pr3x5MlQ1ZAGC+nuZB+EYdgRZgiwxhTBTkF7CXvN" crossorigin="anonymous"></script>
  </body>
</html>

```

articles/templates/details.html:

```py
{% extends 'base.html' %}

{% block title %}{% endblock title %}

{% block style %}{% endblock style %}

{% block body %}
    <div class="container mt-4">
        <h1>{{article.title}}</h1>
        <h6>Published {{article.published}} by <i>{{article.author}}</i></h6>
        <br>
        <p>{{article.description}}</p>
    </div>
{% endblock body %}
```

![](https://user-images.githubusercontent.com/32337103/216662571-196525d5-33c2-4fa9-9908-3af531d9de89.png)

</details>

<details>
  <summary>19. Django Forms - Form vs Model Form </summary>

Form -

```py
from django import forms
from django.contrib.auth.forms import UserCreationForm
from django.contrib.auth.models import User


# Create your forms here.
class NameForm(forms.Form):
    your_name = forms.CharField(label='Your name', max_length=100)
```

```py
from django import forms

class ContactForm(forms.Form):
    name = forms.CharField(max_length=100)
    email = forms.EmailField()
    message = forms.CharField(max_length=1000)
```

```py
from django import forms

class ContactForm(forms.Form):
    subject = forms.CharField(max_length=100)
    message = forms.CharField(widget=forms.Textarea)
    sender = forms.EmailField()
    cc_myself = forms.BooleanField(required=False)
```

```py
from django import forms

class AuthorForm(forms.Form):
    name = forms.CharField(max_length=100)
    title = forms.CharField(
        max_length=3,
        widget=forms.Select(choices=TITLE_CHOICES),
    )
    birth_date = forms.DateField(required=False)

class BookForm(forms.Form):
    name = forms.CharField(max_length=100)
    authors = forms.ModelMultipleChoiceField(queryset=Author.objects.all())
```

```py
from django.core.mail import send_mail

if form.is_valid():
    subject = form.cleaned_data['subject']
    message = form.cleaned_data['message']
    sender = form.cleaned_data['sender']
    cc_myself = form.cleaned_data['cc_myself']

    recipients = ['info@example.com']
    if cc_myself:
        recipients.append(sender)

    send_mail(subject, message, sender, recipients)
    return HttpResponseRedirect('/thanks/')
```

Example:

```py
from django import forms

class NameForm(forms.Form):
    your_name = forms.CharField(label='Your name', max_length=100)
```

```py
<form action="/your-name/" method="post">
    {% csrf_token %}
    {{ form }}
    <input type="submit" value="Submit">
</form>
```

```py
from django.http import HttpResponseRedirect
from django.shortcuts import render

from .forms import NameForm

def get_name(request):
    # if this is a POST request we need to process the form data
    if request.method == 'POST':
        # create a form instance and populate it with data from the request:
        form = NameForm(request.POST)
        # check whether it's valid:
        if form.is_valid():
            # process the data in form.cleaned_data as required
            # ...
            # redirect to a new URL:
            return HttpResponseRedirect('/thanks/')

    # if a GET (or any other method) we'll create a blank form
    else:
        form = NameForm()

    return render(request, 'name.html', {'form': form})
```

Model Form -

```py
from django.forms import ModelForm
from myapp.models import Article

# Create the form class.
class ArticleForm(ModelForm):
    class Meta:
        model = Article
        fields = ['pub_date', 'headline', 'content', 'reporter']

# Creating a form to add an article.
>>> form = ArticleForm()

# Creating a form to change an existing article.
>>> article = Article.objects.get(pk=1)
>>> form = ArticleForm(instance=article)
```

```py
from django.db import models
from django.forms import ModelForm

TITLE_CHOICES = [
    ('MR', 'Mr.'),
    ('MRS', 'Mrs.'),
    ('MS', 'Ms.'),
]

class Author(models.Model):
    name = models.CharField(max_length=100)
    title = models.CharField(max_length=3, choices=TITLE_CHOICES)
    birth_date = models.DateField(blank=True, null=True)

    def __str__(self):
        return self.name

class Book(models.Model):
    name = models.CharField(max_length=100)
    authors = models.ManyToManyField(Author)

class AuthorForm(ModelForm):
    class Meta:
        model = Author
        fields = ['name', 'title', 'birth_date']

class BookForm(ModelForm):
    class Meta:
        model = Book
        fields = ['name', 'authors']
```

```py
from django.core.exceptions import NON_FIELD_ERRORS
from django.forms import ModelForm

class ArticleForm(ModelForm):
    class Meta:
        error_messages = {
            NON_FIELD_ERRORS: {
                'unique_together': "%(model_name)s's %(field_labels)s are not unique.",
            }
        }
```

```py
>>> from myapp.models import Article
>>> from myapp.forms import ArticleForm

# Create a form instance from POST data.
>>> f = ArticleForm(request.POST)

# Save a new Article object from the form's data.
>>> new_article = f.save()

# Create a form to edit an existing Article, but use
# POST data to populate the form.
>>> a = Article.objects.get(pk=1)
>>> f = ArticleForm(request.POST, instance=a)
>>> f.save()
```

```py
# Create a form instance with POST data.
>>> f = AuthorForm(request.POST)

# Create, but don't save the new author instance.
>>> new_author = f.save(commit=False)

# Modify the author in some way.
>>> new_author.some_field = 'some_value'

# Save the new instance.
>>> new_author.save()

# Now, save the many-to-many data for the form.
>>> f.save_m2m()
```

```py
# Create a form instance with POST data.
>>> a = Author()
>>> f = AuthorForm(request.POST, instance=a)

# Create and save the new author instance. There's no need to do anything else.
>>> new_author = f.save()
```

```py
from django.forms import ModelForm

class AuthorForm(ModelForm):
    class Meta:
        model = Author
        fields = '__all__'
```

```py
class PartialAuthorForm(ModelForm):
    class Meta:
        model = Author
        exclude = ['title']
```

```py
author = Author(title='Mr')
form = PartialAuthorForm(request.POST, instance=author)
form.save()

form = PartialAuthorForm(request.POST)
author = form.save(commit=False)
author.title = 'Mr'
author.save()
```

```py
from django.forms import ModelForm, Textarea
from myapp.models import Author

class AuthorForm(ModelForm):
    class Meta:
        model = Author
        fields = ('name', 'title', 'birth_date')
        widgets = {
            'name': Textarea(attrs={'cols': 80, 'rows': 20}),
        }
```

```py
from django.utils.translation import gettext_lazy as _

class AuthorForm(ModelForm):
    class Meta:
        model = Author
        fields = ('name', 'title', 'birth_date')
        labels = {
            'name': _('Writer'),
        }
        help_texts = {
            'name': _('Some useful help text.'),
        }
        error_messages = {
            'name': {
                'max_length': _("This writer's name is too long."),
            },
        }

```

```py
from django.forms import ModelForm
from myapp.models import Article

class ArticleForm(ModelForm):
    class Meta:
        model = Article
        fields = ['pub_date', 'headline', 'content', 'reporter', 'slug']
        field_classes = {
            'slug': MySlugFormField,
        }
```

```py
from django.forms import CharField, ModelForm
from myapp.models import Article

class ArticleForm(ModelForm):
    slug = CharField(validators=[validate_slug])

    class Meta:
        model = Article
        fields = ['pub_date', 'headline', 'content', 'reporter', 'slug']
```

```py
class Article(models.Model):
    headline = models.CharField(
        max_length=200,
        null=True,
        blank=True,
        help_text='Use puns liberally',
    )
    content = models.TextField()

class ArticleForm(ModelForm):
    headline = MyFormField(
        max_length=200,
        required=False,
        help_text='Use puns liberally',
    )

    class Meta:
        model = Article
        fields = ['headline', 'content']
```

```py
>>> article = Article.objects.get(pk=1)
>>> article.headline
'My headline'
>>> form = ArticleForm(initial={'headline': 'Initial headline'}, instance=article)
>>> form['headline'].value()
'Initial headline'
```

```py
>>> from django.forms import modelform_factory
>>> from myapp.models import Book
>>> BookForm = modelform_factory(Book, fields=("author", "title"))

>>> from django.forms import Textarea
>>> Form = modelform_factory(Book, form=BookForm,
                          widgets={"title": Textarea()})
```

```py
from django import forms

class ProductForm(forms.ModelForm):
  	### here date is the field name in the model ###
    date = forms.DateTimeField(widget=forms.DateInput(attrs={'class': 'form-control'}))
    class Meta:
        model = Product
        fields = "__all__"

############## or ##############

class ProductForm(forms.ModelForm):
    class Meta:
        model = Product
        fields = "__all__"

    def __init__(self, *args, **kwargs):
        super(ProductForm, self).__init__(*args, **kwargs)
        self.fields['date'].widget.attrs["class"] = "form-control"

############## or ##############

class ProductForm(forms.ModelForm):
    class Meta:
        model = Product
        fields = "__all__"
		widgets = {
            'date': forms.DateInput(attrs={'class': 'form-control'})
        }
### you can use the attrs to style the fields ###
```

</details>

<details>
  <summary>20. Create Login System with Django Forms</summary>

articles/forms.py:

```py
from django import forms
from django.utils.html import format_html

class LoginForm (forms.Form):
    username = forms.CharField(
        label='Username',
        initial='',
        required=True,
        max_length=50,
        help_text=format_html('<span class="red">50 characters max.</span>'),
        widget=forms.TextInput(attrs={'placeholder': 'Username'}))
    password = forms.CharField(
        label='Password',
        required=True,
        max_length=50,
        help_text=format_html('<span class="red">50 characters max.</span>'),
        widget=forms.PasswordInput(attrs={'placeholder': 'Password'}))
```

articles/views.py:

```py
from django.shortcuts import render, HttpResponse, get_object_or_404
from django.contrib.auth import authenticate, login

from .models import Article
from .forms import LoginForm

# Create your views here.

def article_list(request):
    articles = Article.objects.all().order_by('-published')
    return render(request, 'articles.html', {'articles':articles})

def article_details(request, slug):
    article = get_object_or_404(Article, slug=slug)
    return render(request, 'details.html', {'article':article})

def user_login(request):
    if request.method == 'POST':
        form = LoginForm(request.POST)

        if form.is_valid():
            cd = form.cleaned_data
            user = authenticate(request, username=cd['username'], password=cd['password'])

            if user is None:
                return HttpResponse("Invalid Login")
            login(request, user)
            return HttpResponse("You are authenticated")
    else:
        form = LoginForm()
        # form.fields['username'].initial = ''
    return render(request, 'account/login.html', {'form':form})

```

articles/templates/account/login.html:

```py
{% extends "base.html" %}

{% block title %}Login{% endblock title %}

{% block style %}
    <style>
        .red {
            color: red;
        }
    </style>
{% endblock style %}

{% block body %}
<div class="container">
    <h1>Login User</h1>
    <p>Use the following form for the login</p>
    <form action="" method="post" novalidate>
        {% csrf_token %}
        {{form.as_p}}
        <input type="submit" value="Login" class="btn btn-success">
    </form>
</div>
{% endblock body %}
```

articles/urls.py:

```py
from django.urls import path
from .views import article_list, article_details, user_login

urlpatterns = [
    path('articles/', article_list, name='article_list'),
    path('articles/<slug:slug>/', article_details, name='article_details'),
    path('login/', user_login, name='login')
]
```

![](https://user-images.githubusercontent.com/32337103/216727185-7ee4ce9e-415d-4133-802a-599418db98f9.png)

</details>

<details>
  <summary>21. Install Django Crispy Forms Library</summary>

```bs
pip install django-crispy-forms
```

```py
# In Settings
INSTALLED_APPS = [
	'crispy_forms',
]

# Very Bottom of Settings
CRISPY_TEMPLATE_PACK = 'bootstrap4'

# Top of HTML page
{% load crispy_forms_tags %}

# Apply styling to form
{{ form | crispy }}
```

Crispy-Bootstrap5 -

```bs
### 1- install library ###
pip install crispy-bootstrap5
```

```py
### 2- add the following 2 lines to installed apps in settings.py  ###
"crispy_forms",
"crispy_bootstrap5",

### 3- add the following 2 lines in settings.py ###
CRISPY_ALLOWED_TEMPLATE_PACKS = "bootstrap5"
CRISPY_TEMPLATE_PACK = "bootstrap5"

### 4- now load crispy forms tags and use crispy filter in your html file  ###
{% extends "main/base.html" %}

{% load crispy_forms_tags %}

{% block content %}
<form method="post">
    {% csrf_token %}
    {{form|crispy}}
</form>

{% endblock %}
```

Crispy-Tailwind -

```bs
### install library ###
pip install crispy-tailwind
```

```py
INSTALLED_APPS = (
    ...
    "crispy_forms",
    "crispy_tailwind",
    ...
)

CRISPY_ALLOWED_TEMPLATE_PACKS = "tailwind"

CRISPY_TEMPLATE_PACK = "tailwind"

### Now load crispy forms tags and use crispy filter in your html file  ###
{% extends "main/base.html" %}
{% load tailwind_filters %}

{% block content %}
<form method="post">
    {% csrf_token %}
    {{form|crispy}}
</form>

{% endblock %}
```

</details>

<details>
  <summary>22. Create Registration System with ModelForm and Crispy Forms</summary>

```py
pip install crispy-bootstrap5
```

```py
# Application definition

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'articles',
    "crispy_forms",
    "crispy_bootstrap5",
]
```

```py
CRISPY_ALLOWED_TEMPLATE_PACKS = "bootstrap5"
CRISPY_TEMPLATE_PACK = "bootstrap5"
```

articles/forms.py:

```py
from django import forms
from django.utils.html import format_html
from django.contrib.auth.models import User

class LoginForm(forms.Form):
    username = forms.CharField(
        label='Username',
        initial='',
        required=True,
        max_length=50,
        help_text=format_html('<span class="red">50 characters max.</span>'),
        widget=forms.TextInput(attrs={'placeholder': 'Username'}))
    password = forms.CharField(
        label='Password',
        required=True,
        max_length=50,
        help_text=format_html('<span class="red">50 characters max.</span>'),
        widget=forms.PasswordInput(attrs={'placeholder': 'Password'}))

class UserRegistration(forms.ModelForm):
    password = forms.CharField(label='Password', widget=forms.PasswordInput)
    password2 = forms.CharField(label='Confirm Password', widget=forms.PasswordInput)

    class Meta:
        model = User
        fields = ('username', 'first_name', 'email')

    def clean_password2(self):
        cd = self.cleaned_data
        if cd['password'] != cd['password2']:
            raise forms.ValidationError('Passwords do not match')
        else:
            return cd['password2']

```

articles/views.py:

```py
from django.shortcuts import render, HttpResponse, get_object_or_404
from django.contrib.auth import authenticate, login

from .models import Article
from .forms import LoginForm, UserRegistration


# Create your views here.

def article_list(request):
    articles = Article.objects.all().order_by('-published')
    return render(request, 'articles.html', {'articles':articles})

def article_details(request, slug):
    article = get_object_or_404(Article, slug=slug)
    return render(request, 'details.html', {'article':article})

def user_login(request):
    if request.method == 'POST':
        form = LoginForm(request.POST)

        if form.is_valid():
            cd = form.cleaned_data
            user = authenticate(request, username=cd['username'], password=cd['password'])

            if user is None:
                return HttpResponse("Invalid Login")
            login(request, user)
            return HttpResponse("You are authenticated")
    else:
        form = LoginForm()
        # form.fields['username'].initial = ''
    return render(request, 'account/login.html', {'form':form})

def register(request):
    if request.method == 'POST':
        user_form = UserRegistration(request.POST)

        if user_form.is_valid():
            new_user = user_form.save(commit=False)
            new_user.set_password(user_form.cleaned_data['password'])
            new_user.save()
            return render(request, 'account/register_done.html', {'user_form': user_form})
    else:
        user_form = UserRegistration()
    return render(request, 'account/register.html', {'user_form': user_form})

```

articles/urls.py:

```py
from django.urls import path
from .views import article_list, article_details, user_login, register

urlpatterns = [
    path('articles/', article_list, name='article_list'),
    path('articles/<slug:slug>/', article_details, name='article_details'),
    path('login/', user_login, name='login'),
    path('register/', register, name='register'),
]
```

articles/templates/account/register.html:

```py
{% extends "base.html" %}

{% load crispy_forms_tags %}

{% block title %}Registration{% endblock title %}

{% block style %}
    <style>
        .register_style {
            width: 500px;
            margin: auto;
        }
    </style>
{% endblock style %}

{% block body %}
<div class="container my-4 register_style">
    <h3>Register an account </h3>
    <p>Use the following form for Registration.</p>
    <form action="" method="post" novalidate>
        {% csrf_token %}
        {{user_form | crispy}}
        <input type="submit" value="Create Account" class="btn btn-success">
    </form>
</div>
{% endblock body %}

```

articles/templates/account/register_done.html:

```py
{% extends "base.html" %}

{% block title %}Done{% endblock title %}

{% block body %}
<div class="container">
    <h3>Welcome, {{new_user.username}}</h3>
    <p>Your Account has been created, you can <a href="{% url 'login' %}">Login Here</a>.</p>
</div>
{% endblock body %}
```

![](https://user-images.githubusercontent.com/32337103/216756087-a47a6683-66cd-4b10-97d8-6af375e03456.png)

![](https://user-images.githubusercontent.com/32337103/216756103-6ea9235c-3843-440e-9b44-9e1df060e055.png)

![](https://user-images.githubusercontent.com/32337103/216756124-b462ce08-2eca-4933-a324-8314b3e72364.png)

![](https://user-images.githubusercontent.com/32337103/216756132-585e7bd2-6517-4636-8fa6-d690ffa91632.png)

![](https://user-images.githubusercontent.com/32337103/216756149-2055825a-9340-4549-ae57-2fcdb9c30f6d.png)

</details>

<details>
  <summary>23. Use Django Class LoginView for Login </summary>

djblog/articles/urls.py:

```py
from django.urls import path
from .views import article_list, article_details, user_login, register
from django.contrib.auth.views import LoginView

urlpatterns = [
    path('articles/', article_list, name='article_list'),
    path('articles/<slug:slug>/', article_details, name='article_details'),
    # path('login/', user_login, name='login'),
    path('login/', LoginView.as_view(), name='login'),
    path('register/', register, name='register'),
]
```

djblog/djblog/settings.py:

```py

# Internationalization
# https://docs.djangoproject.com/en/4.1/topics/i18n/

LANGUAGE_CODE = 'en-us'

TIME_ZONE = 'UTC'

USE_I18N = True

USE_TZ = True

LOGIN_REDIRECT_URL = 'article_list'
LOGIN_URL = 'login'
LOGOUT_URL = 'logout'
```

djblog/articles/templates/registration/login.html:

```py
{% extends "base.html" %}

{% load crispy_forms_tags %}

{% block title %}Login{% endblock title %}

{% block style %}
    <style>
        .red {
            color: red;
        }
        .login_style {
            width: 500px;
            margin: auto;
        }
    </style>
{% endblock style %}

{% block body %}
<div class="container my-4 login_style">
    <h1>Login User</h1>

    {% if form.errors %}
        <p class="red">Incorrect Username or Password!</p>
    {% else %}
        <p>Use the following form below for the login.</p>
    {% endif  %}
    <form action="" method="post" novalidate>
        {% csrf_token %}
        {{form | crispy}}
        <input type="hidden" name="next" value="{{next}}" />
        <input type="submit" value="Login" class="btn btn-success" />
    </form>
</div>
{% endblock body %}

```

![](https://user-images.githubusercontent.com/32337103/216801732-ff7d1432-44c2-483d-9bda-2304270cdacf.png)

![](https://user-images.githubusercontent.com/32337103/216801734-f5cd4dd0-20c5-4a1b-8e4d-ae1648d73fbe.png)

</details>

<details>
  <summary>24. Use Django Class LogoutView for Logout </summary>

djblog/articles/urls.py:

```py
from django.urls import path
from .views import article_list, article_details, user_login, register
from django.contrib.auth.views import LoginView, LogoutView

urlpatterns = [
    path('articles/', article_list, name='article_list'),
    path('articles/<slug:slug>/', article_details, name='article_details'),
    # path('login/', user_login, name='login'),
    path('login/', LoginView.as_view(), name='login'),
    path('logout/', LogoutView.as_view(), name='logout'),
    path('register/', register, name='register'),
]
```

djblog/djblog/settings.py:

```py
# Application definition

INSTALLED_APPS = [
    'articles', # raise app to the first item in the list
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    "crispy_forms",
    "crispy_bootstrap5",
]

```

djblog/articles/templates/registration/logged_out.html:

```py
{% extends "base.html" %}

{% block title %}Logged out{% endblock title %}

{% block body %}
<div class="container my-4">
    <p>You have been logged out.</p>
    <p>You can <a href="{% url 'login' %}">Login Back</a>.</p>

</div>
{% endblock body %}
```

![](https://user-images.githubusercontent.com/32337103/216802222-c873061a-a9d4-4b1d-a570-0c3e2e31c3a4.png)

![](https://user-images.githubusercontent.com/32337103/216802235-9e0636d5-1274-4d57-8fdf-a978b0b45608.png)

![](https://user-images.githubusercontent.com/32337103/216802250-ba0c5ea3-1375-40bb-a008-825323e408dc.png)

</details>

<details>
  <summary>25. Restricting/Hiding menu in Navbar with user.is_authenticated </summary>

```html
<li class="nav-item">
  {% if request.user.is_anonymous %}
  <a class="nav-link disabled">Welcome, Visitor.</a>
  {% else %}
  <a class="nav-link disabled">Welcome, {{request.user.username | title}}.</a>
  {% endif %}
</li>
```

djblog/articles/templates/navbar.html:

```py
<nav class="navbar navbar-expand-lg" style="background-color: #e3f2fd;">
    <div class="container">
      <a class="navbar-brand" href="{% url 'article_list' %}">Django-Blog</a>
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNavDropdown" aria-controls="navbarNavDropdown" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarNavDropdown">
        <ul class="navbar-nav">

            {% if request.user.is_authenticated %}

            <li class="nav-item mx-5">
                <a class="nav-link disabled">Welcome, {{request.user.username | title}}.</a>
            </li>
            <li class="nav-item">
                <a class="nav-link active" aria-current="page" href="{% url 'article_list' %}">Home</a>
            </li>
            <li class="nav-item">
                <a class="nav-link" href="#">Add Article</a>
            </li>
            <li class="nav-item dropdown">
                <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                Profile
                </a>
                <ul class="dropdown-menu">
                <li><a class="dropdown-item" href="#">Change Password</a></li>
                <li><a class="dropdown-item" href="{% url 'logout' %}">Logout</a></li>
                </ul>
            </li>

            {% else %}

            <li class="nav-item mx-5">
              <a class="nav-link disabled">Welcome, Visitor.</a>
            </li>
            <li class="nav-item">
                <a class="nav-link" href="{% url 'login' %}">Login</a>
            </li>
            <li class="nav-item">
                <a class="nav-link" href="{% url 'register' %}">Register</a>
            </li>

            {% endif %}
        </ul>
      </div>
    </div>
  </nav>
```

![](https://user-images.githubusercontent.com/32337103/216804185-0b81f25d-d0cb-48f9-8776-25d7c71cde73.png)

![](https://user-images.githubusercontent.com/32337103/216804194-c965dd1a-7e19-4efa-9f4b-67391e5a800a.png)

</details>

<details>
  <summary>26. Password Change with PasswordChangeView </summary>

djblog/articles/urls.py:

```py
from django.urls import path
from .views import article_list, article_details, user_login, register
from django.contrib.auth.views import (LoginView, LogoutView, PasswordChangeView,
                                       PasswordChangeDoneView)

urlpatterns = [
    path('articles/', article_list, name='article_list'),
    path('articles/<slug:slug>/', article_details, name='article_details'),
    # path('login/', user_login, name='login'),
    path('login/', LoginView.as_view(), name='login'),
    path('logout/', LogoutView.as_view(), name='logout'),
    path('register/', register, name='register'),
    path('password_change/', PasswordChangeView.as_view(), name='password_change'),
    path('password_change/done/', PasswordChangeDoneView.as_view(), name='password_change_done'),
]

# from django.urls import path, include
# from django.contrib.auth import views as auth_views
# from . import views
# from django.urls import reverse_lazy

# app_name = 'account'

# urlpatterns = [
#     #Password Changes URLs
#     path('password_change/', auth_views.PasswordChangeView.as_view(
#         success_url=reverse_lazy('account:password_change_done')
#     ), name='password_change'),
#     path('password_change/done/',auth_views.PasswordChangeDoneView.as_view(), name='password_change_done'),
# ]
```

djblog/articles/templates/registration/password_change_form.html:

```py
{% extends "base.html" %}

{% load crispy_forms_tags %}

{% block title %}Password Change{% endblock title %}

{% block style %}
    <style>
        .register_style {
            width: 500px;
            margin: auto;
        }
    </style>
{% endblock style %}

{% block body %}
<div class="container my-5 register_style">
    <h3>Change Your Password </h3>
    <form action="" method="post" novalidate>
        {% csrf_token %}
        {{form | crispy}}
        <input type="submit" value="Change Password" class="btn btn-success" />
    </form>
</div>
{% endblock body %}
```

djblog/articles/templates/registration/password_change_done.html:

```py
{% extends "base.html" %}

{% block title %}Password Change Done{% endblock title %}

{% block body %}
<div class="container my-5">
    <h3>Password Changed </h3>
    <p>Your password has been changed!</p>
</div>
{% endblock body %}
```

djblog/articles/templates/navbar.html:

```py
<nav class="navbar navbar-expand-lg" style="background-color: #e3f2fd;">
    <div class="container">
      <a class="navbar-brand" href="{% url 'article_list' %}">Django-Blog</a>
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNavDropdown" aria-controls="navbarNavDropdown" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarNavDropdown">
        <ul class="navbar-nav">

            {% if request.user.is_authenticated %}

            <li class="nav-item mx-5">
                <a class="nav-link disabled">Welcome, {{request.user.username | title}}.</a>
            </li>
            <li class="nav-item">
                <a class="nav-link active" aria-current="page" href="{% url 'article_list' %}">Home</a>
            </li>
            <li class="nav-item">
                <a class="nav-link" href="#">Add Article</a>
            </li>
            <li class="nav-item dropdown">
                <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                Profile
                </a>
                <ul class="dropdown-menu">
                <li><a class="dropdown-item" href="{% url 'password_change' %}">Change Password</a></li>
                <li><a class="dropdown-item" href="{% url 'logout' %}">Logout</a></li>
                </ul>
            </li>

            {% else %}

            <li class="nav-item mx-5">
              <a class="nav-link disabled">Welcome, Visitor.</a>
            </li>
            <li class="nav-item">
                <a class="nav-link" href="{% url 'login' %}">Login</a>
            </li>
            <li class="nav-item">
                <a class="nav-link" href="{% url 'register' %}">Register</a>
            </li>

            {% endif %}
        </ul>
      </div>
    </div>
  </nav>
```

![](https://user-images.githubusercontent.com/32337103/216840877-9cba86b3-6a0e-4ea0-b08c-17d7e95f54a8.png)

![](https://user-images.githubusercontent.com/32337103/216840894-cfc8b155-60c1-4589-848b-0cd8cf671cfe.png)

![](https://user-images.githubusercontent.com/32337103/216840932-488bb0dc-f53b-4812-8858-a76d11a2ce75.png)

</details>

<details>
  <summary>27. Add Article with ArticleRegistrationForm </summary>

djblog/articles/forms.py:

```py
from django import forms
from django.utils.html import format_html
from django.contrib.auth.models import User
from .models import Article

class LoginForm(forms.Form):
    username = forms.CharField(
        label='Username',
        initial='',
        required=True,
        max_length=50,
        help_text=format_html('<span class="red">50 characters max.</span>'),
        widget=forms.TextInput(attrs={'placeholder': 'Username'}))
    password = forms.CharField(
        label='Password',
        required=True,
        max_length=50,
        help_text=format_html('<span class="red">50 characters max.</span>'),
        widget=forms.PasswordInput(attrs={'placeholder': 'Password'}))

class UserRegistration(forms.ModelForm):
    password = forms.CharField(label='Password', widget=forms.PasswordInput)
    password2 = forms.CharField(label='Confirm Password', widget=forms.PasswordInput)

    class Meta:
        model = User
        fields = ('username', 'first_name', 'email')

    def clean_password2(self):
        cd = self.cleaned_data
        if cd['password'] != cd['password2']:
            raise forms.ValidationError('Passwords do not match')
        else:
            return cd['password2']

class ArticleRegistrationForm(forms.ModelForm):
    class Meta:
        model = Article
        fields = ('title', 'description')
```

djblog/articles/views.py:

```py
from django.shortcuts import render, HttpResponse, get_object_or_404, redirect
from django.contrib.auth import authenticate, login

from .models import Article
from .forms import LoginForm, UserRegistration, ArticleRegistrationForm


# Create your views here.

def article_list(request):
    articles = Article.objects.all().order_by('-published')
    return render(request, 'articles.html', {'articles':articles})

def article_details(request, slug):
    article = get_object_or_404(Article, slug=slug)
    return render(request, 'details.html', {'article':article})

def user_login(request):
    if request.method == 'POST':
        form = LoginForm(request.POST)

        if form.is_valid():
            cd = form.cleaned_data
            user = authenticate(request, username=cd['username'], password=cd['password'])

            if user is None:
                return HttpResponse("Invalid Login")
            login(request, user)
            return HttpResponse("You are authenticated")
    else:
        form = LoginForm()
        # form.fields['username'].initial = ''
    return render(request, 'account/login.html', {'form':form})

def register(request):
    if request.method == 'POST':
        user_form = UserRegistration(request.POST)

        if user_form.is_valid():
            new_user = user_form.save(commit=False)
            new_user.set_password(user_form.cleaned_data['password'])
            new_user.save()
            return render(request, 'account/register_done.html', {'user_form': user_form})
    else:
        user_form = UserRegistration()
    return render(request, 'account/register.html', {'user_form': user_form})

def article_form(request):
    if not request.user.is_authenticated:
        return redirect('article_list') # <--- Protect Route for Add Article
    if request.method == 'POST':
        article_form = ArticleRegistrationForm(request.POST)

        if article_form.is_valid():
            article = article_form.save(commit=False)
            article.author = request.user
            article.save()
            return redirect('article_list')
    else:
        article_form = ArticleRegistrationForm()
    return render(request, 'account/add_article.html', {'article_form': article_form})

```

djblog/articles/templates/account/add_article.html:

```py
{% extends "base.html" %}

{% load crispy_forms_tags %}

{% block title %}Add Article{% endblock title %}

{% block style %}
    <style>
        .article_style {
            width: 500px;
            margin: auto;
        }
    </style>
{% endblock style %}

{% block body %}
<div class="container mt-4 article_style">
    <h1>Add Article</h1>
    <p>Use the following form below to add an Article.</p>
    <form action="" method="post" novalidate>
        {% csrf_token %}
        {{article_form | crispy}}
        <input type="submit" value="Add Article" class="btn btn-success">
    </form>
</div>
{% endblock body %}

```

djblog/articles/urls.py:

```py
from django.urls import path
from .views import article_list, article_details, user_login, register, article_form
from django.contrib.auth.views import (LoginView, LogoutView, PasswordChangeView,
                                       PasswordChangeDoneView)

urlpatterns = [
    path('articles/', article_list, name='article_list'),
    path('articles/<slug:slug>/', article_details, name='article_details'),
    path('add/', article_form, name='article_form'), # <---- Add Article
    # path('login/', user_login, name='login'),
    path('login/', LoginView.as_view(), name='login'),
    path('logout/', LogoutView.as_view(), name='logout'),
    path('register/', register, name='register'),
    path('password_change/', PasswordChangeView.as_view(), name='password_change'),
    path('password_change/done/', PasswordChangeDoneView.as_view(), name='password_change_done'),
]

```

![](https://user-images.githubusercontent.com/32337103/216846064-d648a52d-7c9b-4817-913c-74cf64131447.png)

![](https://user-images.githubusercontent.com/32337103/216846089-7d2df4d1-dc28-4d87-82f0-2c88b9ea77cf.png)

</details>

<details>
  <summary>28. Update Article with ArticleUpdateForm </summary>

djblog/articles/forms.py:

```py
from django import forms
from django.utils.html import format_html
from django.contrib.auth.models import User
from .models import Article

class LoginForm(forms.Form):
    username = forms.CharField(
        label='Username',
        initial='',
        required=True,
        max_length=50,
        help_text=format_html('<span class="red">50 characters max.</span>'),
        widget=forms.TextInput(attrs={'placeholder': 'Username'}))
    password = forms.CharField(
        label='Password',
        required=True,
        max_length=50,
        help_text=format_html('<span class="red">50 characters max.</span>'),
        widget=forms.PasswordInput(attrs={'placeholder': 'Password'}))

class UserRegistration(forms.ModelForm):
    password = forms.CharField(label='Password', widget=forms.PasswordInput)
    password2 = forms.CharField(label='Confirm Password', widget=forms.PasswordInput)

    class Meta:
        model = User
        fields = ('username', 'first_name', 'email')

    def clean_password2(self):
        cd = self.cleaned_data
        if cd['password'] != cd['password2']:
            raise forms.ValidationError('Passwords do not match')
        else:
            return cd['password2']

class ArticleRegistrationForm(forms.ModelForm):
    class Meta:
        model = Article
        fields = ('title', 'description')

class ArticleUpdateForm(forms.ModelForm):
    class Meta:
        model = Article
        fields = ('title', 'description')
```

djblog/articles/views.py:

```py
from django.shortcuts import render, HttpResponse, get_object_or_404, redirect
from django.contrib.auth import authenticate, login

from .models import Article
from .forms import LoginForm, UserRegistration, ArticleRegistrationForm, ArticleUpdateForm


# Create your views here.

def article_list(request):
    articles = Article.objects.all().order_by('-published')
    return render(request, 'articles.html', {'articles':articles})

def article_details(request, slug):
    article = get_object_or_404(Article, slug=slug)
    return render(request, 'details.html', {'article':article})

def user_login(request):
    if request.method == 'POST':
        form = LoginForm(request.POST)

        if form.is_valid():
            cd = form.cleaned_data
            user = authenticate(request, username=cd['username'], password=cd['password'])

            if user is None:
                return HttpResponse("Invalid Login")
            login(request, user)
            return HttpResponse("You are authenticated")
    else:
        form = LoginForm()
        # form.fields['username'].initial = ''
    return render(request, 'account/login.html', {'form':form})

def register(request):
    if request.method == 'POST':
        user_form = UserRegistration(request.POST)

        if user_form.is_valid():
            new_user = user_form.save(commit=False)
            new_user.set_password(user_form.cleaned_data['password'])
            new_user.save()
            return render(request, 'account/register_done.html', {'user_form': user_form})
    else:
        user_form = UserRegistration()
    return render(request, 'account/register.html', {'user_form': user_form})

def article_form(request):
    if not request.user.is_authenticated:
        return redirect('article_list') # <--- Protect Route for Add Article
    if request.method == 'POST':
        article_form = ArticleRegistrationForm(request.POST)

        if article_form.is_valid():
            article = article_form.save(commit=False)
            article.author = request.user
            article.save()
            return redirect('article_list')
    else:
        article_form = ArticleRegistrationForm()
    return render(request, 'account/add_article.html', {'article_form': article_form})


def update_article(request, slug):
    article = get_object_or_404(Article, slug=slug)
    form = ArticleUpdateForm(request.POST or None, instance=article)

    if form.is_valid():
        form.save()
        return redirect('article_list')
    return render(request, 'account/update.html', {'form': form})
```

djblog/articles/urls.py:

```py
from django.urls import path
from .views import (article_list, article_details, user_login, register,
                    article_form, update_article)
from django.contrib.auth.views import (LoginView, LogoutView, PasswordChangeView,
                                       PasswordChangeDoneView)

urlpatterns = [
    path('articles/', article_list, name='article_list'),
    path('articles/<slug:slug>/', article_details, name='article_details'),
    path('add/', article_form, name='article_form'),
    path('update/<slug:slug>/', update_article, name='update_article'),
    # path('login/', user_login, name='login'),
    path('login/', LoginView.as_view(), name='login'),
    path('logout/', LogoutView.as_view(), name='logout'),
    path('register/', register, name='register'),
    path('password_change/', PasswordChangeView.as_view(), name='password_change'),
    path('password_change/done/', PasswordChangeDoneView.as_view(), name='password_change_done'),
]

```

djblog/articles/templates/account/update.html:

```py
{% extends "base.html" %}

{% load crispy_forms_tags %}

{% block title %}{{form.title}}{% endblock title %}

{% block style %}
    <style>
        .update_style {
            width: 700px;
            margin: auto;
        }
    </style>
{% endblock style %}

{% block body %}
<div class="container my-4 update_style">
    <h3>Update Article </h3>
    <p>Use the following form to update the Article.</p>
    <form action="" method="post" novalidate>
        {% csrf_token %}
        {{form | crispy}}
        <input type="submit" value="Update Article" class="btn btn-success">
    </form>
</div>
{% endblock body %}

```

djblog/articles/templates/details.html:

```py
{% extends 'base.html' %}

{% block title %}{% endblock title %}

{% block style %}{% endblock style %}

{% block body %}
    <div class="container mt-4">
        <h1>{{article.title}}</h1>
        <h6>Published {{article.published}} by <i>{{article.author}}</i></h6>
        <br>
        <p>{{article.description}}</p>

        {% if request.user == article.author %}
        <a class="btn btn-danger mx-3 mt-3" href="">Delete</a>
        <a class="btn btn-success mx-3 mt-3" href="{% url 'update_article' article.slug %}">Update</a>
        {% endif %}
    </div>
{% endblock body %}
```

![](https://user-images.githubusercontent.com/32337103/216848734-2ec4c3ae-58fc-495a-a5f5-d8289876475e.png)

![](https://user-images.githubusercontent.com/32337103/216848744-bd133608-c568-4229-8e56-53af614f2bf7.png)

![](https://user-images.githubusercontent.com/32337103/216848774-4f3a7fe8-3964-4485-b2a6-b0817697a97c.png)

![](https://user-images.githubusercontent.com/32337103/216848806-06fb89ae-c05b-4199-88e6-33c3b0184dbd.png)

</details>

<details>
  <summary>29. Delete Article </summary>

djblog/articles/views.py:

```py
from django.shortcuts import render, HttpResponse, get_object_or_404, redirect
from django.contrib.auth import authenticate, login

from .models import Article
from .forms import LoginForm, UserRegistration, ArticleRegistrationForm, ArticleUpdateForm


def article_list(request):
    articles = Article.objects.all().order_by('-published')
    return render(request, 'articles.html', {'articles':articles})

def article_details(request, slug):
    article = get_object_or_404(Article, slug=slug)
    return render(request, 'details.html', {'article':article})

def user_login(request):
    if request.method == 'POST':
        form = LoginForm(request.POST)

        if form.is_valid():
            cd = form.cleaned_data
            user = authenticate(request, username=cd['username'], password=cd['password'])

            if user is None:
                return HttpResponse("Invalid Login")
            login(request, user)
            return HttpResponse("You are authenticated")
    else:
        form = LoginForm()
        # form.fields['username'].initial = ''
    return render(request, 'account/login.html', {'form':form})

def register(request):
    if request.method == 'POST':
        user_form = UserRegistration(request.POST)

        if user_form.is_valid():
            new_user = user_form.save(commit=False)
            new_user.set_password(user_form.cleaned_data['password'])
            new_user.save()
            return render(request, 'account/register_done.html', {'user_form': user_form})
    else:
        user_form = UserRegistration()
    return render(request, 'account/register.html', {'user_form': user_form})

def article_form(request):
    if not request.user.is_authenticated:
        return redirect('article_list') # <--- Protect Route for Add Article
    if request.method == 'POST':
        article_form = ArticleRegistrationForm(request.POST)

        if article_form.is_valid():
            article = article_form.save(commit=False)
            article.author = request.user
            article.save()
            return redirect('article_list')
    else:
        article_form = ArticleRegistrationForm()
    return render(request, 'account/add_article.html', {'article_form': article_form})


def update_article(request, slug):
    article = get_object_or_404(Article, slug=slug)
    form = ArticleUpdateForm(request.POST or None, instance=article)

    if form.is_valid():
        form.save()
        return redirect('article_list')
    return render(request, 'account/update.html', {'form': form})

def delete_article(request, slug):
    article = get_object_or_404(Article, slug=slug)
    article.delete()
    return redirect('article_list')
```

djblog/articles/urls.py:

```py
from django.urls import path
from .views import (article_list, article_details, user_login, register,
                    article_form, update_article, delete_article)
from django.contrib.auth.views import (LoginView, LogoutView, PasswordChangeView,
                                       PasswordChangeDoneView)

urlpatterns = [
    path('articles/', article_list, name='article_list'),
    path('articles/<slug:slug>/', article_details, name='article_details'),
    path('add/', article_form, name='article_form'),
    path('update/<slug:slug>/', update_article, name='update_article'),
    path('delete/<slug:slug>/', delete_article, name='delete_article'),
    # path('login/', user_login, name='login'),
    path('login/', LoginView.as_view(), name='login'),
    path('logout/', LogoutView.as_view(), name='logout'),
    path('register/', register, name='register'),
    path('password_change/', PasswordChangeView.as_view(), name='password_change'),
    path('password_change/done/', PasswordChangeDoneView.as_view(), name='password_change_done'),
]

```

djblog/articles/templates/details.html:

```py
{% extends 'base.html' %}

{% block title %}{% endblock title %}

{% block style %}{% endblock style %}

{% block body %}
    <div class="container mt-4">
        <h1>{{article.title}}</h1>
        <h6>Published {{article.published}} by <i>{{article.author}}</i></h6>
        <br>
        <p>{{article.description}}</p>

        {% if request.user == article.author %}
        <a class="btn btn-danger mx-3 mt-3" href="{% url 'delete_article' article.slug %}">Delete</a>
        <a class="btn btn-success mx-3 mt-3" href="{% url 'update_article' article.slug %}">Update</a>
        {% endif %}
    </div>
{% endblock body %}
```

![](https://user-images.githubusercontent.com/32337103/216925835-de1396f5-a5c9-4639-af24-40fe46c72164.png)

![](https://user-images.githubusercontent.com/32337103/216925897-d5c9a5c9-2ecf-4635-96f7-ec867ed27fc4.png)

![](https://user-images.githubusercontent.com/32337103/216925943-ad390b72-9243-4351-bf99-6530cf64d4cc.png)

</details>

<details>
  <summary>30. Django Pagination </summary>

```bs
def article_list(request):
    article_list = Article.objects.all().order_by('-published')
    paginator = Paginator(article_list, 2)
    page = request.GET.get('page')

    try:
        articles = paginator.page(page)
    except PageNotAnInteger:
        articles = paginator.page(1)
    except EmptyPage:
        articles = paginator.page(paginator.num_pages)

    return render(request, 'articles.html', {'articles':articles, 'page': page})
```

djblog/articles/views.py:

```py
from django.shortcuts import render, HttpResponse, get_object_or_404, redirect
from django.contrib.auth import authenticate, login
from django.core.paginator import Paginator, EmptyPage, PageNotAnInteger

from .models import Article
from .forms import LoginForm, UserRegistration, ArticleRegistrationForm, ArticleUpdateForm


# Create your views here.

def article_list(request):
    article_list = Article.objects.all().order_by('-published')
    paginator = Paginator(article_list, 2)
    page = request.GET.get('page')

    try:
        articles = paginator.page(page)
    except PageNotAnInteger:
        articles = paginator.page(1)
    except EmptyPage:
        articles = paginator.page(paginator.num_pages)

    return render(request, 'articles.html', {'articles':articles, 'page': page})

def article_details(request, slug):
    article = get_object_or_404(Article, slug=slug)
    return render(request, 'details.html', {'article':article})

def user_login(request):
    if request.method == 'POST':
        form = LoginForm(request.POST)

        if form.is_valid():
            cd = form.cleaned_data
            user = authenticate(request, username=cd['username'], password=cd['password'])

            if user is None:
                return HttpResponse("Invalid Login")
            login(request, user)
            return HttpResponse("You are authenticated")
    else:
        form = LoginForm()
        # form.fields['username'].initial = ''
    return render(request, 'account/login.html', {'form':form})

def register(request):
    if request.method == 'POST':
        user_form = UserRegistration(request.POST)

        if user_form.is_valid():
            new_user = user_form.save(commit=False)
            new_user.set_password(user_form.cleaned_data['password'])
            new_user.save()
            return render(request, 'account/register_done.html', {'user_form': user_form})
    else:
        user_form = UserRegistration()
    return render(request, 'account/register.html', {'user_form': user_form})

def article_form(request):
    if not request.user.is_authenticated:
        return redirect('article_list') # <--- Protect Route for Add Article
    if request.method == 'POST':
        article_form = ArticleRegistrationForm(request.POST)

        if article_form.is_valid():
            article = article_form.save(commit=False)
            article.author = request.user
            article.save()
            return redirect('article_list')
    else:
        article_form = ArticleRegistrationForm()
    return render(request, 'account/add_article.html', {'article_form': article_form})


def update_article(request, slug):
    article = get_object_or_404(Article, slug=slug)
    form = ArticleUpdateForm(request.POST or None, instance=article)

    if form.is_valid():
        form.save()
        return redirect('article_list')
    return render(request, 'account/update.html', {'form': form})

def delete_article(request, slug):
    article = get_object_or_404(Article, slug=slug)
    article.delete()
    return redirect('article_list')
```

djblog/articles/templates/account/pagination.html:

```py
<style type="text/css">
    .paginator,
    .paginator-next,
    .paginator-prev {
        color:white;
        background:black;
        font-size:12px;
        padding: 10px;
        text-decoration: none;
        font-weight: bold;
    }
    .paginator-next {
        border-radius: 0 10px 10px 0;
        background:gray;
    }
    .paginator-prev {
        border-radius: 10px 0 0 10px;
        background:gray;
    }
</style>

<nav aria-label="page navigation example">
    <ul class="pagination" style="margin-top: 30px;">
        <li class="page-item">
            {% if page.has_previous %}
            <a class="paginator-prev" href="?page={{page.previous_page_number}}">Previous</a>
            {% comment %} class="page-link" {% endcomment %}
            {% endif %}
        </li>

        <li class="page-item">
            <a class="paginator">Page {{page.number}} of {{page.paginator.num_pages}}</a>
        </li>

        <li class="page-item" >
            {% if page.has_next %}
            <a class="paginator-next" href="?page={{page.next_page_number}}">Next</a>
            {% endif %}
        </li>
    </ul>
</nav>
```

djblog/articles/templates/articles.html:

```py
{% extends 'base.html' %}

{% block title %} Article List {% endblock title%}

{% block style %}
<style>
    .h1-style {
        margin: 2rem 0;
        color: #2c9df0;
    }
    .link-style {
        text-decoration: none;
        color:black;
    }
    .link-style:hover {
        text-decoration: none;
        color:gray;
    }
</style>
{% endblock style %}

{%block body %}
<section class="container">
    <h1 class="h1-style">Article List</h1>
    {% for article in articles %}
        <span class="badge rounded-pill text-bg-success">Author: {{article.author}}</span>
        <h1><a class="link-style" href="{% url 'article_details' article.slug %}">{{article.title}}</a></h1>
    {% endfor %}

    {% include 'account/pagination.html' with page=articles %}

</section>

{% endblock body %}

```

![](https://user-images.githubusercontent.com/32337103/216942190-717c3fe3-a503-4609-8cb1-acb8abcabf2d.png)

![](https://user-images.githubusercontent.com/32337103/216942243-a567777e-aebf-4eba-9273-0f5daa3b55da.png)

</details>

<details>
  <summary>31. Using Login_required Decorator to restrict access to pages </summary>

djblog/articles/urls.py:

```py
from django.urls import path
from .views import (article_list, article_details, user_login, register,
                    article_form, update_article, delete_article)
from django.contrib.auth.views import (LoginView, LogoutView, PasswordChangeView,
                                       PasswordChangeDoneView)

urlpatterns = [
    path('', article_list, name='article_list'),
    path('articles/<slug:slug>/', article_details, name='article_details'),
    path('add/', article_form, name='article_form'),
    path('update/<slug:slug>/', update_article, name='update_article'),
    path('delete/<slug:slug>/', delete_article, name='delete_article'),
    # path('login/', user_login, name='login'),
    path('login/', LoginView.as_view(), name='login'),
    path('logout/', LogoutView.as_view(), name='logout'),
    path('register/', register, name='register'),
    path('password_change/', PasswordChangeView.as_view(), name='password_change'),
    path('password_change/done/', PasswordChangeDoneView.as_view(), name='password_change_done'),
]

```

```bs
@login_required
def article_form(request):
    # if not request.user.is_authenticated:
    #     return redirect('article_list') # <--- Protect Route for Add Article
    if request.method == 'POST':
        article_form = ArticleRegistrationForm(request.POST)

        if article_form.is_valid():
            article = article_form.save(commit=False)
            article.author = request.user
            article.save()
            return redirect('article_list')
    else:
        article_form = ArticleRegistrationForm()
    return render(request, 'account/add_article.html', {'article_form': article_form})
```

djblog/articles/views.py:

```py
from django.shortcuts import render, HttpResponse, get_object_or_404, redirect
from django.contrib.auth import authenticate, login
from django.core.paginator import Paginator, EmptyPage, PageNotAnInteger
from django.contrib.auth.decorators import login_required

from .models import Article
from .forms import LoginForm, UserRegistration, ArticleRegistrationForm, ArticleUpdateForm


# Create your views here.

def article_list(request):
    article_list = Article.objects.all().order_by('-published')
    paginator = Paginator(article_list, 2)
    page = request.GET.get('page')

    try:
        articles = paginator.page(page)
    except PageNotAnInteger:
        articles = paginator.page(1)
    except EmptyPage:
        articles = paginator.page(paginator.num_pages)

    return render(request, 'articles.html', {'articles':articles, 'page': page})

def article_details(request, slug):
    article = get_object_or_404(Article, slug=slug)
    return render(request, 'details.html', {'article':article})

def user_login(request):
    if request.method == 'POST':
        form = LoginForm(request.POST)

        if form.is_valid():
            cd = form.cleaned_data
            user = authenticate(request, username=cd['username'], password=cd['password'])

            if user is None:
                return HttpResponse("Invalid Login")
            login(request, user)
            return HttpResponse("You are authenticated")
    else:
        form = LoginForm()
        # form.fields['username'].initial = ''
    return render(request, 'account/login.html', {'form':form})

def register(request):
    if request.method == 'POST':
        user_form = UserRegistration(request.POST)

        if user_form.is_valid():
            new_user = user_form.save(commit=False)
            new_user.set_password(user_form.cleaned_data['password'])
            new_user.save()
            return render(request, 'account/register_done.html', {'user_form': user_form})
    else:
        user_form = UserRegistration()
    return render(request, 'account/register.html', {'user_form': user_form})

@login_required
def article_form(request):
    # if not request.user.is_authenticated:
    #     return redirect('article_list') # <--- Protect Route for Add Article
    if request.method == 'POST':
        article_form = ArticleRegistrationForm(request.POST)

        if article_form.is_valid():
            article = article_form.save(commit=False)
            article.author = request.user
            article.save()
            return redirect('article_list')
    else:
        article_form = ArticleRegistrationForm()
    return render(request, 'account/add_article.html', {'article_form': article_form})

@login_required
def update_article(request, slug):
    article = get_object_or_404(Article, slug=slug)
    form = ArticleUpdateForm(request.POST or None, instance=article)

    if form.is_valid():
        form.save()
        return redirect('article_list')
    return render(request, 'account/update.html', {'form': form})

@login_required
def delete_article(request, slug):
    article = get_object_or_404(Article, slug=slug)
    article.delete()
    return redirect('article_list')
```

![](https://user-images.githubusercontent.com/32337103/216984681-9345b074-47ba-4a96-8345-924af1f3c498.png)

</details>

<details>
  <summary>32. Deploy Django App to Heroku </summary>

Install Dependencies/Modules:

```py
pip install whitenoise
```

```py
pip install gunicorn
```

Create requirements.txt file:

```py
pip freeze > requirements.txt
```

Configure Settings.py file for Production:

```py
pip install python-dotenv
```

```bs
import os
from dotenv import load_dotenv
load_dotenv()  # loads the configs from .env

if 'SECRET_KEY' in os.environ:
    SECRET_KEY = os.environ["SECRET_KEY"]

DEBUG = False

ALLOWED_HOSTS = [] if DEBUG else ["*"]
```

Make sure staticfiles is configured correctly:

```bs
STATIC_ROOT = BASE_DIR / "staticfiles"
STATIC_URL = "static/"
STATICFILES_STORAGE = "whitenoise.storage.CompressedManifestStaticFilesStorage"

# STATIC_ROOT = os.path.join(BASE_DIR, '/staticfiles')
# MEDIA_ROOT = os.path.join(BASE_DIR, "media/")
# STATICFILES_DIRS = [ BASE_DIR / "articles/static"]
```

```bs
{% load static %}
<img src="{% static "images/hi.jpg" %}" alt="Hi!">
```

Enable WhiteNoise:

```bs
MIDDLEWARE = [
    # ...
    "django.middleware.security.SecurityMiddleware",
    "whitenoise.middleware.WhiteNoiseMiddleware",
    # ...
]
```

Add compression and caching support:

```bs
STORAGES = {
    # ...
    "staticfiles": {
        "BACKEND": "whitenoise.storage.CompressedManifestStaticFilesStorage",
    },
}
```

```bs
STATICFILES_STORAGE = "whitenoise.storage.CompressedManifestStaticFilesStorage"
```

Using WhiteNoise in development:

- You can disable Django’s static file handling and allow WhiteNoise to take over simply by passing the --nostatic option to the runserver command, but you need to remember to add this option every time you call runserver.
- An easier way is to edit your settings.py file and add whitenoise.runserver_nostatic to the top of your INSTALLED_APPS list:

```bs
INSTALLED_APPS = [
    # ...
    "whitenoise.runserver_nostatic",
    "django.contrib.staticfiles",
    # ...
]
```

djblog/djblog/settings.py:

```py
"""
Django settings for djblog project.

Generated by 'django-admin startproject' using Django 4.1.5.

For more information on this file, see
https://docs.djangoproject.com/en/4.1/topics/settings/

For the full list of settings and their values, see
https://docs.djangoproject.com/en/4.1/ref/settings/
"""

import os
from pathlib import Path
from dotenv import load_dotenv
load_dotenv()  # loads the configs from .env

# Build paths inside the project like this: BASE_DIR / 'subdir'.
BASE_DIR = Path(__file__).resolve().parent.parent

# Quick-start development settings - unsuitable for production
# See https://docs.djangoproject.com/en/4.1/howto/deployment/checklist/

# SECURITY WARNING: keep the secret key used in production secret!
if 'SECRET_KEY' in os.environ:
    SECRET_KEY = os.environ["SECRET_KEY"]
    # SECRET_KEY = str(os.getenv('SECRET_KEY'))

# SECURITY WARNING: don't run with debug turned on in production!
DEBUG = False

ALLOWED_HOSTS = [] if DEBUG else ["*"]

# Application definition

INSTALLED_APPS = [
    'articles',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    "crispy_forms",
    "crispy_bootstrap5",
]

MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'whitenoise.middleware.WhiteNoiseMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]

ROOT_URLCONF = 'djblog.urls'

TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]

WSGI_APPLICATION = 'djblog.wsgi.application'


# Database
# https://docs.djangoproject.com/en/4.1/ref/settings/#databases

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}

STORAGES = {
    "staticfiles": {
        "BACKEND": "whitenoise.storage.CompressedManifestStaticFilesStorage",
    },
}

# Password validation
# https://docs.djangoproject.com/en/4.1/ref/settings/#auth-password-validators

AUTH_PASSWORD_VALIDATORS = [
    {
        'NAME': 'django.contrib.auth.password_validation.UserAttributeSimilarityValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.MinimumLengthValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.CommonPasswordValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.NumericPasswordValidator',
    },
]


# Internationalization
# https://docs.djangoproject.com/en/4.1/topics/i18n/

LANGUAGE_CODE = 'en-us'

TIME_ZONE = 'UTC'

USE_I18N = True

USE_TZ = True

LOGIN_REDIRECT_URL = 'article_list'
LOGIN_URL = 'login'
LOGOUT_URL = 'logout'

# Static files (CSS, JavaScript, Images)
# https://docs.djangoproject.com/en/4.1/howto/static-files/


STATIC_ROOT = BASE_DIR / "staticfiles"
STATIC_URL = "static/"
STATICFILES_STORAGE = "whitenoise.storage.CompressedManifestStaticFilesStorage"
# STATIC_ROOT = os.path.join(BASE_DIR, '/static')
# MEDIA_ROOT = os.path.join(BASE_DIR, "media/")
# STATICFILES_DIRS = [ BASE_DIR / "articles/static"]

# Default primary key field type
# https://docs.djangoproject.com/en/4.1/ref/settings/#default-auto-field

DEFAULT_AUTO_FIELD = 'django.db.models.BigAutoField'

CRISPY_ALLOWED_TEMPLATE_PACKS = "bootstrap5"
CRISPY_TEMPLATE_PACK = "bootstrap5"

```

djblog/articles/static/css/index.css:

```css
body {
  background: #ccced6;
  /* background: #000; */
}

.h1-style {
  margin: 2rem 0;
  color: #003d5b;
}

.link-style {
  text-decoration: none;
  color: black;
}

.link-style:hover {
  text-decoration: none;
  color: gray;
}

.article_style {
  width: 500px;
  margin: auto;
}

.register_style {
  width: 500px;
  margin: auto;
}

.update_style {
  width: 700px;
  margin: auto;
}

.red {
  color: red;
}

.paginator,
.paginator-next,
.paginator-prev {
  color: white;
  background: black;
  font-size: 12px;
  padding: 10px;
  text-decoration: none;
  font-weight: bold;
}
.paginator-next {
  border-radius: 0 10px 10px 0;
  background: gray;
}
.paginator-prev {
  border-radius: 10px 0 0 10px;
  background: gray;
}
```

djblog/articles/templates/articles.html

```py
{% extends 'base.html' %}

{% block title %} Article List {% endblock title%}

{% block style %}{% endblock style %}

{%block body %}
<section class="container">
    <h1 class="h1-style">Articles</h1>
    {% for article in articles %}
        <span class="badge rounded-pill text-bg-success">Author: {{article.author}}</span>
        <h1><a class="link-style" href="{% url 'article_details' article.slug %}">{{article.title}}</a></h1>
    {% endfor %}

    {% include 'account/pagination.html' with page=articles %}

</section>

{% endblock body %}
```

Run Collectstatic to collect static files:

```bs
python manage.py collectstatic
```

Install the Heroku CLI:

```bs
brew tap heroku/brew && brew install heroku
brew install heroku
brew upgrade heroku
```

Check Installed Heroku Version:

```bs
heroku --version
```

Login to Heroku Account:

```bs
heroku login
```

Check Heroku Account details:

```bs
heroku auth:whoami
heroku auth:token
heroku authorizations
heroku authorizations:info 059ed27c-d04a-4349-9dba-83a0169277ae
```

```bs
Create .gitignore file
```

```bs
git init
git add .
git commit -m "Initial commit"
```

Create Heroku app:

```bs
heroku create
#Creating app... done, ⬢ rocky-wildwood-72845

heroku create <project-name>
```

Check remote Heroku apps:

```bs
heroku apps
```

Set remote Heroku app location:

```bs
heroku git:remote -a rocky-wildwood-72845
heroku git:remote --app rocky-wildwood-72845
```

```bs
create Procfile
```

Procfile:

```bs
web: gunicorn djblog.wsgi
```

Choose the Python Version (optional):

```bs
echo python-3.9.6 > runtime.txt
git add runtime.txt
git commit -m "Request a specific Python version"
```

Make final commit and push to Heroku:

```bs
git add .
git commit -m "Created Procfile"
git push heroku master
```

```bs
https://rocky-wildwood-72845.herokuapp.com/ deployed to Heroku
```

</details>

<details>
  <summary>33. Protect Secrets with Python-dotenv </summary>

```bs
pip install python-dotenv
```

.env:

```py
SECRET_KEY = 'YOUR SECRET KEY'

GITHUB_KEY = 'YOUR GITHUB KEY'
GITHUB_SECRET = 'YOUR GITHUB SECRET KEY'

GOOGLE_KEY = 'YOUR GOOGLE KEY'
GOOGLE_SECRET = 'YOUR GOOGLE SECRET KEY'
```

settings.py:

```py
import os
from dotenv import load_dotenv
load_dotenv()  # loads the configs from .env

# SECURITY WARNING: keep the secret key used in production secret!
# SECRET_KEY = str(os.getenv('SECRET_KEY'))
SECRET_KEY = os.environ["SECRET_KEY"]

# social auth configs for github
#SOCIAL_AUTH_GITHUB_KEY = str(os.getenv('GITHUB_KEY'))
#SOCIAL_AUTH_GITHUB_SECRET = str(os.getenv('GITHUB_SECRET'))
SECRET_KEY = os.environ["GITHUB_KEY"]
SECRET_KEY = os.environ["GITHUB_SECRET"]

# social auth configs for google
#SOCIAL_AUTH_GOOGLE_OAUTH2_KEY = str(os.getenv('GOOGLE_KEY'))
#SOCIAL_AUTH_GOOGLE_OAUTH2_SECRET = str(os.getenv('GOOGLE_SECRET'))
SECRET_KEY = os.environ["GOOGLE_KEY"]
SECRET_KEY = os.environ["GOOGLE_SECRET"]
```

</details>

<details>
  <summary>34. Set Heroku Config .env Values </summary>

View current configuration values:

```py
heroku config
```

```py
# GITHUB_USERNAME: joesmith
# OTHER_VAR:    production
```

Get a single configuration value:

```py
heroku config:get GITHUB_USERNAME
```

```py
# joesmith
```

Set/add a configuration value:

```py
heroku config:set GITHUB_USERNAME=joesmith
```

```py
# Adding config vars and restarting myapp... done, v12
# GITHUB_USERNAME: joesmith
```

Remove a configuration value:

```py
heroku config:unset GITHUB_USERNAME
```

```py
# Unsetting GITHUB_USERNAME and restarting myapp... done, v13
```

</details>

<details>
  <summary>35. Renaming Heroku Remote App </summary>

```py
heroku apps:rename newname
```

```py
# Renaming oldname to newname... done
# http://newname.herokuapp.com/ | git@herokuapp.com:newname.git
# Git remote heroku updated
```

```py
heroku apps:rename newname --app oldname
```

```py
# http://newname.herokuapp.com/ | git@herokuapp.com:newname.git
```

Updating Git remotes:

```py
git remote rm heroku
heroku git:remote -a newname
```

```py
# If you're working on the heroku remote (default):
heroku git:remote -a [app name]

# If you want to specify a different remote, use the -r argument:
heroku git:remote -a [app name] -r [remote]

# Delete the current remote reference with
git remote rm origin

# Add the new remote
git remote add origin <URL to new heroku app>

# push to new domain
git push -u origin master

# View Remote URLs
git remote -v

# Remove Heroku remote URL
git remote rm heroku

# Set new Heroku URL
heroku git:remote -a  ############

git remote set-url heroku <repo git>
git remote -v
```

</details>

<details>
  <summary>36. Migrating to PostgreSQL </summary>

Install Python Adapter for Database Migration:

```py
pip install psycopg2
python -m pip install psycopg2-binary
```

```py
pip freeze > requirements.txt
```

Install Postgresql:

```py
https://postgresapp.com/
```

Install pgAdmin:

```py
https://www.pgadmin.org/
```

```py
psql --version
psql -U postgres

# List Databases:
\list
\l

# List Tables:
\dt

# Quit terminal:
\q
```

Settings for Postgresql connection:

```py
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'db_name',
        'USER': 'db_user',
        'PASSWORD': 'db_user_password',
        'HOST': '',
        'PORT': 'db_port_number',
    }
}
```

```py
# Just after the default DATABASE configuration add this in settings.py

POSTGRES_DB = os.environ.get("POSTGRES_DB")
POSTGRES_PASSWORD = os.environ.get("POSTGRES_PASSWORD")
POSTGRES_USER = os.environ.get("POSTGRES_USER")
POSTGRES_HOST = os.environ.get("POSTGRES_HOST")
POSTGRES_PORT = os.environ.get("POSTGRES_PORT")

POSTGRES_READY = (
    POSTGRES_DB is not None
    and POSTGRES_PASSWORD is not None
    and POSTGRES_USER is not None
    and POSTGRES_HOST is not None
    and POSTGRES_PORT is not None
)

if POSTGRES_READY:
    DATABASES = {
        "default": {
            "ENGINE": "django.db.backends.postgresql",
            "NAME": POSTGRES_DB,
            "USER": POSTGRES_USER,
            "PASSWORD": POSTGRES_PASSWORD,
            "HOST": POSTGRES_HOST,
            "PORT": POSTGRES_PORT,
        }
    }

```

Settings for LOCAL Postgresql connection:

```py
DATABASES = {
    # 'default': {
    #     'ENGINE': 'django.db.backends.sqlite3',
    #     'NAME': BASE_DIR / 'db.sqlite3',
    # }

    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'postgres',
        'USER': '',
        'PASSWORD': '',
        'HOST': 'localhost',
        'PORT': 5432,
    }
}
```

```py
python manage.py createsuperuser
```

![](https://user-images.githubusercontent.com/32337103/217245031-7cf49cc6-bfe2-400b-a536-b6e3fcdda90c.png)
![](https://user-images.githubusercontent.com/32337103/217245135-7b117e31-d69b-47d5-9e39-62afbfb7ea5f.png)

Settings for REMOTE Postgresql connection (with Railway):

```py
DATABASES = {
    # 'default': {
    #     'ENGINE': 'django.db.backends.sqlite3',
    #     'NAME': BASE_DIR / 'db.sqlite3',
    # }

    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': os.environ.get("PGDATABASE"),
        'USER': os.environ.get("PGUSER"),
        'PASSWORD': os.environ.get("PGPASSWORD"),
        'HOST':  os.environ.get("PGHOST"),
        'PORT': os.environ.get("PGPORT"),
    }
}
```

```py
python manage.py migrate
```

```py
git add .
git commit -m "Connected to Postgresql"
git push heroku master
```

</details>

+BUILDING QUESTION AND ANSWER APP IN DJANGO

<details>
  <summary>37. Introduction </summary>

Install Venv:

```py
python -m venv venv_djqa
source venv_djqa/bin/activate
```

Install Django

```py
pip install Django
```

Upgrade pip:

```py
python -m pip install --upgrade pip
```

Create Django Project:

```py
django-admin startproject djqa .
```

Create Users app:

```py
python manage.py startapp users
```

</details>

<details>
  <summary>38. Create CustomUser Model </summary>

djqa/users/models.py:

```py
from django.db import models
from django.contrib.auth.models import AbstractUser

# Create your models here.
class CustomUser(AbstractUser):
    pass
```

djqa/djqa/settings.py:

```py
# Application definition

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'users',
]

#Set Model User
AUTH_USER_MODEL = 'users.CustomUser'

```

Make migrations:

```py
python manage.py makemigrations

```

```py
# Migrations for 'users':
#   users/migrations/0001_initial.py
#     - Create model CustomUser
```

Run Migration:

```py
python manage.py migrate
```

```py
# Operations to perform:
#   Apply all migrations: admin, auth, contenttypes, sessions, users
# Running migrations:
#   Applying contenttypes.0001_initial... OK
#   Applying contenttypes.0002_remove_content_type_name... OK
#   Applying auth.0001_initial... OK
#   Applying auth.0002_alter_permission_name_max_length... OK
#   Applying auth.0003_alter_user_email_max_length... OK
#   Applying auth.0004_alter_user_username_opts... OK
#   Applying auth.0005_alter_user_last_login_null... OK
#   Applying auth.0006_require_contenttypes_0002... OK
#   Applying auth.0007_alter_validators_add_error_messages... OK
#   Applying auth.0008_alter_user_username_max_length... OK
#   Applying auth.0009_alter_user_last_name_max_length... OK
#   Applying auth.0010_alter_group_name_max_length... OK
#   Applying auth.0011_update_proxy_permissions... OK
#   Applying auth.0012_alter_user_first_name_max_length... OK
#   Applying users.0001_initial... OK
#   Applying admin.0001_initial... OK
#   Applying admin.0002_logentry_remove_auto_add... OK
#   Applying admin.0003_logentry_add_action_flag_choices... OK
#   Applying sessions.0001_initial... OK
```

Run server:

```py
python manage.py runserver
```

```py
# Watching for file changes with StatReloader
# Performing system checks...

# System check identified no issues (0 silenced).
# February 10, 2023 - 19:58:17
# Django version 4.1.6, using settings 'djqa.settings'
# Starting development server at http://127.0.0.1:8000/
# Quit the server with CONTROL-C.
```

![](https://user-images.githubusercontent.com/32337103/218186250-0178f098-5e14-4ebb-b34f-da5eb23d043f.png)

Create Super User:

```py
python manage.py createsuperuser
```

```py
# Username: admin
# Email address: admin@gmail.com
# Password:
# Password (again):
# This password is too short. It must contain at least 8 characters.
# This password is too common.
# This password is entirely numeric.
# Bypass password validation and create user anyway? [y/N]: y
# Superuser created successfully.
```

http://127.0.0.1:8000/admin:

![](https://user-images.githubusercontent.com/32337103/218187218-279ccd88-243a-478c-b682-84ede19e3b5a.png)

![](https://user-images.githubusercontent.com/32337103/218187292-cfc00c72-3d3f-49c6-961a-c5476d028ec2.png)

Add CustomUser Model to Admin -

djqa/users/admin.py:

```py
from django.contrib import admin
from .models import CustomUser

# Register your models here.
admin.site.register(CustomUser)
```

![](https://user-images.githubusercontent.com/32337103/218187813-f2473d5d-3f35-4cc3-9937-f701f770d1da.png)

![](https://user-images.githubusercontent.com/32337103/218188052-fc6cbcaa-3bb9-4d80-ad17-273fa9e996d9.png)

</details>

<details>
  <summary>39. Create Questions App</summary>

```py
python manage.py startapp questions
```

djqa/djqa/settings.py:

```py
# Application definition

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'users',
    'questions',
]
```

</details>

<details>
  <summary>40. Create Question and Answer Models </summary>

djqa/questions/models.py:

```py
from django.db import models
from django.conf import settings

# Create your models here.
class Question(models.Model):
    title= models.CharField(max_length=250)
    body = models.TextField()
    slug = models.SlugField(max_length=250, unique=True)
    author = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE,related_name='questions')
    created_at = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.title

class Answer(models.Model):
    description = models.TextField()
    question = models.ForeignKey(Question, on_delete=models.CASCADE, related_name='answers')
    author = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    created_at = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.author.username
```

Make Migrations:

```py
python manage.py makemigrations
```

```py
# Migrations for 'questions':
#   questions/migrations/0001_initial.py
#     - Create model Question
#     - Create model Answer
```

Run Migration:

```py
python manage.py migrate
```

```py
# Operations to perform:
#   Apply all migrations: admin, auth, contenttypes, questions, sessions, users
# Running migrations:
#   Applying questions.0001_initial... OK
```

Add Models to Admin -

djqa/questions/admin.py:

```py
from django.contrib import admin
from .models import Question, Answer

# Register your models here.
admin.site.register(Question)
admin.site.register(Answer)
```

![](https://user-images.githubusercontent.com/32337103/218191785-7d0e6793-1bc1-4df2-872b-ed70301fe856.png)

</details>

<details>
  <summary>41. Create dynamic Slugs for Questions with signals </summary>

djqa/questions/signals.py:

```py
from django.db.models.signals import pre_save
from django.dispatch import receiver
from .models import Question
from django.utils.text import slugify

@receiver(pre_save, sender=Question)
def add_slug(sender, instance, *args, **kwargs):
    if instance and not instance.slug:
        slug = slugify(instance.title)
        instance.slug = slug
```

djqa/questions/apps.py:

```py
from django.apps import AppConfig


class QuestionsConfig(AppConfig):
    default_auto_field = 'django.db.models.BigAutoField'
    name = 'questions'

    def ready(self):
        import questions.signals
```

djqa/questions/init.py:

```py
default_app_config = "questions.apps.QuestionsConfig"
```

Open Python Shell:

```py
python manage.py shell
```

```py
# from django.contrib.auth import get_user_model
# user = get_user_model()
# u = user.objects.first()
# from questions.models import Question
# q = Question(title = "This is my question", body = 'This is the question body', author=u)
# q.save()
# exit()
```

```py
python manage.py runserver
```

![](https://user-images.githubusercontent.com/32337103/218195342-7b5c8e51-91c1-478d-be6d-b083ca31d9fd.png)

![](https://user-images.githubusercontent.com/32337103/218195435-74ce77ab-b58c-4086-a3c6-b206ef733573.png)

</details>

<details>
  <summary>42. Render Question List Page </summary>

djqa/djqa/urls.py:

```py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('questions.urls'))
]

```

djqa/questions/urls.py:

```py
from django.urls import path
from .views import question_list

urlpatterns = [
    path('question/', question_list, name='question_list')
]
```

djqa/questions/views.py:

```py
from django.shortcuts import render
from .models import Question

# Create your views here.
def question_list(request):
    question_list = Question.objects.all().order_by('-created_at')
    return render(request, 'questionList.html', {'question_list': question_list})

```

djqa/templates/questionList.html:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Question List</title>
  </head>
  <body>
    <h1>Question 1</h1>
    <p>What is the Capital of Nigeria?</p>
  </body>
</html>
```

djqa/djqa/settings.py:

```py
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': ['templates'],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```

![](https://user-images.githubusercontent.com/32337103/218429996-5ff4a211-a5bf-4c6b-8adf-83d382e75b91.png)

</details>

<details>
  <summary>43. Modify Template with Bootstrap </summary>

djqa/templates/base.html:

```py
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{% block title %}{% endblock title %}</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-GLhlTQ8iRABdZLl6O3oVMWSktQOp6b7In1Zl3/Jr59b6EGGoI1aFkw7cmDA6j6gD" crossorigin="anonymous">
    {% block style %}{% endblock style %}
</head>
<body>
    {% block body %}{% endblock body %}
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/js/bootstrap.bundle.min.js" integrity="sha384-w76AqPfDkMBDXo30jS1Sgez6pr3x5MlQ1ZAGC+nuZB+EYdgRZgiwxhTBTkF7CXvN" crossorigin="anonymous"></script>
</body>
</html>
```

djqa/templates/questionList.html:

```py
{% extends 'base.html' %}

{% block title %}Question List{% endblock title %}

{% block style%}{% endblock style%}

{% block body%}
<div class="container">
{% for question in question_list %}
    <div class="card mt-3 shadow">
        <div class="card-body">
            <h5 class="card-title"><a href="">{{question.title}}</a></h5>
            <p class="card-text">{{question.body}}</p>
        </div>
    </div>
{% endfor %}
</div>
{% endblock body%}
<body>

```

![](https://user-images.githubusercontent.com/32337103/218435813-7cdd2c49-7472-479c-868f-7d0874910c8b.png)

![](https://user-images.githubusercontent.com/32337103/218434831-94970245-a6bb-4e5d-93cb-dfd362ed3c86.png)

djqa/templates/questionList.html:

```py
{% extends 'base.html' %}

{% block title %}Question List{% endblock title %}

{% block style%}
    <style>
        .link-style {
            text-decoration:none;
            color: #00798C;
        }
        .link-style:hover {
            text-decoration:none;
            color:gray;
        }
    </style>
{% endblock style%}

{% block body%}
<div class="container">
{% for question in question_list %}
    <div class="card mt-3 shadow">
        <div class="card-body">
            <h5 class="card-title"><a class="link-style" href="">{{question.title}}</a></h5>
            <p class="card-text">{{question.body}}</p>
        </div>
        <div class="card-footer">
            <div class="row">
                <div class="col col-md-auto">
                    Posted By: {{question.author.username}}
                </div>
                <div class="col col-md-auto">
                    Answers: {{question.answers.count}}
                </div>
            </div>
        </div>
    </div>
{% endfor %}
</div>
{% endblock body%}
<body>

```

djqa/questions/models.py:

```py
from django.db import models
from django.conf import settings

# Create your models here.
class Question(models.Model):
    title= models.CharField(max_length=250)
    body = models.TextField()
    slug = models.SlugField(max_length=250, unique=True)
    author = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE,related_name='questions')
    created_at = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.title

class Answer(models.Model):
    description = models.TextField()
    question = models.ForeignKey(Question, on_delete=models.CASCADE, related_name='answers')
    author = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    created_at = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.author.username
```

![](https://user-images.githubusercontent.com/32337103/218442066-0780b24e-5783-4d8f-8efd-6bd8e6732afb.png)

</details>

<details>
  <summary>44. Adding Bootstrap Navbar </summary>

djqa/templates/navbar.html:

```py
<style>
    .text-style {
        font-size: 30px !important;
        font-family: fantasy !important;
        color: brown !important;
        font-weight: bold !important;
    }
</style>

<nav class="navbar navbar-expand-lg bg-body-tertiary">
    <div class="container-fluid">
      <a class="navbar-brand text-style" href="#">Question Hub</a>
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav me-auto mb-2 mb-lg-0">
          <li class="nav-item">
            <a class="nav-link disabled">Welcome, Alex.</a>
          </li>
          <li class="nav-item">
            <a class="nav-link active" aria-current="page" href="#">Home</a>
          </li>

          <li class="nav-item">
            <a class="nav-link" href="#">Add Question</a>
          </li>

          <li class="nav-item dropdown">
            <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">
              Profile
            </a>
            <ul class="dropdown-menu">
              <li><a class="dropdown-item" href="#">Change Password</a></li>
              <li><a class="dropdown-item" href="#">Change Account</a></li>
              <li><a class="dropdown-item" href="#">Question & Answer</a></li>
              <li><hr class="dropdown-divider"></li>
              <li><a class="dropdown-item" href="#">Logout</a></li>
            </ul>
          </li>

          <li class="nav-item">
            <a class="nav-link" href="#">Login</a>
          </li>

          <li class="nav-item">
            <a class="nav-link" href="#">Register</a>
          </li>
        </ul>
        <form class="d-flex" role="search">
          <input class="form-control me-2" type="search" placeholder="Search" aria-label="Search">
          <button class="btn btn-outline-success" type="submit">Search</button>
        </form>
      </div>
    </div>
  </nav>
```

djqa/templates/base.html:

```py
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{% block title %}{% endblock title %}</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-GLhlTQ8iRABdZLl6O3oVMWSktQOp6b7In1Zl3/Jr59b6EGGoI1aFkw7cmDA6j6gD" crossorigin="anonymous">
    {% block style %}{% endblock style %}
</head>
<body>
    {% include 'navbar.html' %}
    {% block body %}{% endblock body %}
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/js/bootstrap.bundle.min.js" integrity="sha384-w76AqPfDkMBDXo30jS1Sgez6pr3x5MlQ1ZAGC+nuZB+EYdgRZgiwxhTBTkF7CXvN" crossorigin="anonymous"></script>
</body>
</html>
```

![](https://user-images.githubusercontent.com/32337103/218559989-e236eedd-ccfb-4e41-be4b-0e2c43768965.png)

</details>

<details>
  <summary>45. Create Details Page for Questions </summary>

Cloud-Django/djqa/questions/urls.py:

```py
from django.urls import path
from .views import question_list, question_details

urlpatterns = [
    path('question/', question_list, name='question_list'),
    path('question/<slug:slug>/', question_details, name='question_details')
]
```

Cloud-Django/djqa/questions/views.py:

```py
from django.shortcuts import render, get_object_or_404
from .models import Question

# Create your views here.
def question_list(request):
    question_list = Question.objects.all().order_by('-created_at')
    return render(request, 'questionList.html', {'question_list': question_list})

def question_details(request, slug):
    question = get_object_or_404(Question, slug=slug )
    return render(request, 'questionDetails.html', {'question': question})
```

Cloud-Django/djqa/templates/questionDetails.html:

```py
{% extends 'base.html' %}

{% block title %} Question Details {% endblock title %}

{% block body %}
<div class="container mt-3">
    <h1>{{question.title}}</h1>
    <p>{{question.body}}</p>
    <h6>
        Posted By: <i>{{question.author}} </i>
    </h6>
    <p>Published {{question.created_at}}</p>
</div>
{% endblock body %}
```

Cloud-Django/djqa/templates/questionList.html:

```htmlx
<h5 class="card-title"><a class="link-style" href="{% url 'question_details' question.slug %}">{{question.title}}</a></h5>
```

```py
{% extends 'base.html' %}

{% block title %}Question List{% endblock title %}

{% block style%}
    <style>
        .link-style {
            text-decoration:none;
            color: #00798C;
        }
        .link-style:hover {
            text-decoration:none;
            color:gray;
        }
    </style>
{% endblock style%}

{% block body%}
<div class="container">
{% for question in question_list %}
    <div class="card mt-3 shadow">
        <div class="card-body">
            <h5 class="card-title"><a class="link-style" href="{% url 'question_details' question.slug %}">{{question.title}}</a></h5>
            <p class="card-text">{{question.body}}</p>
        </div>
        <div class="card-footer">
            <div class="row">
                <div class="col col-md-auto">
                    Posted By: {{question.author.username}}
                </div>
                <div class="col col-md-auto">
                    Answers: {{question.answers.count}}
                </div>
            </div>
        </div>
    </div>
{% endfor %}
</div>
{% endblock body%}
<body>

```

![](https://user-images.githubusercontent.com/32337103/218563663-b9e44fad-01bc-4ca4-b387-5bd0293d56f4.png)

![](https://user-images.githubusercontent.com/32337103/218563694-58952ba7-0bc1-4a9e-8334-c963858e6025.png)

![](https://user-images.githubusercontent.com/32337103/218563733-d484e09c-139d-48fe-a95b-a7eef7b4adc7.png)

</details>

<details>
  <summary>46. Create Login Page </summary>

```bs
pip install django-crispy-forms
```

```bs
pip install crispy-bootstrap5
```

Cloud-Django/djqa/djqa/settings.py:

```py
# Application definition

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    "crispy_forms",
    "crispy_bootstrap5",
    'users',
    'questions',
]


CRISPY_ALLOWED_TEMPLATE_PACKS = "bootstrap5"
CRISPY_TEMPLATE_PACK = "bootstrap5"

LOGIN_REDIRECT_URL = 'question_list'
LOGIN_URL = 'login'
LOGOUT_URL = 'logout'

```

Cloud-Django/djqa/users/urls.py:

```py
from django.urls import path
from django.contrib.auth.views import LoginView, LogoutView

urlpatterns = [
    path('login/', LoginView.as_view(), name='login'),
    path('logout/', LogoutView.as_view(), name='logout')
]
```

Cloud-Django/djqa/djqa/urls.py:

```py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('questions.urls')),
    path('', include('users.urls'))
]

```

Cloud-Django/djqa/templates/navbar.html:

```bs
<li><a class="dropdown-item" href="{% url 'logout' %}">Logout</a></li>

<a class="nav-link" href="{% url 'login' %}">Login</a>
```

```py
<style>
    .text-style {
        font-size: 30px !important;
        font-family: fantasy !important;
        color: brown !important;
        font-weight: bold !important;
    }
</style>

<nav class="navbar navbar-expand-lg bg-body-tertiary">
    <div class="container-fluid">
      <a class="navbar-brand text-style" href="#">Question Hub</a>
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav me-auto mb-2 mb-lg-0">
          <li class="nav-item">
            <a class="nav-link disabled">Welcome, Alex.</a>
          </li>
          <li class="nav-item">
            <a class="nav-link active" aria-current="page" href="#">Home</a>
          </li>

          <li class="nav-item">
            <a class="nav-link" href="#">Add Question</a>
          </li>

          <li class="nav-item dropdown">
            <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">
              Profile
            </a>
            <ul class="dropdown-menu">
              <li><a class="dropdown-item" href="#">Change Password</a></li>
              <li><a class="dropdown-item" href="#">Change Account</a></li>
              <li><a class="dropdown-item" href="#">Question & Answer</a></li>
              <li><hr class="dropdown-divider"></li>
              <li><a class="dropdown-item" href="{% url 'logout' %}">Logout</a></li>
            </ul>
          </li>

          <li class="nav-item">
            <a class="nav-link" href="{% url 'login' %}">Login</a>
          </li>

          <li class="nav-item">
            <a class="nav-link" href="#">Register</a>
          </li>
        </ul>
        <form class="d-flex" role="search">
          <input class="form-control me-2" type="search" placeholder="Search" aria-label="Search">
          <button class="btn btn-outline-success" type="submit">Search</button>
        </form>
      </div>
    </div>
  </nav>
```

Cloud-Django/djqa/templates/registration/login.html:

```py
{% extends 'base.html' %}
{% load crispy_forms_tags %}


{% block title %} Login {% endblock title %}

{% block style %}
<style>
    .login-style {
        width:500px;
        height: auto;
    }
</style>
{% endblock style %}

{% block body %}
<div class="container mt-4 login-style">
    <h1>Login User</h1>

    <form action="" method="post" novalidate>
        {% csrf_token %}
        {{form | crispy}}

        <input type="hidden" name="text" value="{{next}}"/>
        <input type="submit" value="Login" class="btn btn-success">
    </form>
</div>
{% endblock body %}
```

![](https://user-images.githubusercontent.com/32337103/219012659-9023e028-b4c6-489a-9e69-fd2f21199a41.png)

![](https://user-images.githubusercontent.com/32337103/219013062-8b08cc85-0b54-48d5-99a8-97bf5e20b581.png)

</details>

<details>
  <summary>47. Create Logout Page </summary>

Cloud-Django/djqa/templates/registration/logged_out.html:

```py
{% extends 'base.html' %}

{% block title %} Logout {% endblock title %}

{% block style %}
<style>
    .login-style {
        width:500px;
        height: auto;
    }
</style>
{% endblock style %}

{% block body %}
<div class="container">
    <p>You have been logout. You can <a class="" href="{% url 'login' %}">Login Here</a>.</p>
</div>
{% endblock body %}
```

![](https://user-images.githubusercontent.com/32337103/219015533-9cf4daf1-a9d8-4258-8d17-ca4b81b7d90b.png)

</details>

<details>
  <summary>48. Create User Registration Page </summary>

Cloud-Django/djqa/questions/forms.py:

```py
from django import forms
from django.contrib.auth import get_user_model

User = get_user_model()

class UserRegistrationForm(forms.ModelForm):
    password = forms.CharField(label='Password', widget=forms.PasswordInput)
    password2 = forms.CharField(label='Confirm Password', widget=forms.PasswordInput)

    class Meta:
        model = User
        fields = ('username', 'first_name', 'email')

    def clean_password2(self):
        cd = self.cleaned_data
        if cd['password'] != cd['password2']:
            raise forms.ValidationError('Passwords don\'t match.')
        return cd['password2']
```

Cloud-Django/djqa/questions/views.py:

```py
from django.shortcuts import render, get_object_or_404
from .models import Question
from .forms import UserRegistrationForm

# Create your views here.
def question_list(request):
    question_list = Question.objects.all().order_by('-created_at')
    return render(request, 'questionList.html', {'question_list': question_list})

def question_details(request, slug):
    question = get_object_or_404(Question, slug=slug )
    return render(request, 'questionDetails.html', {'question': question})

def register(request):
    if request.method == "POST":
        user_form = UserRegistrationForm(request.POST)

        if user_form.is_valid():
            new_user = user_form.save(commit=False)
            new_user.set_password(user_form.cleaned_data['password'])
            new_user.save()
            return render(request, 'register_done.html', {'user_form':user_form})
    else:
        user_form = UserRegistrationForm()
    return render(request, 'register.html', {'user_form':user_form})
```

Cloud-Django/djqa/templates/register.html:

```py
{% extends 'base.html' %}
{% load crispy_forms_tags %}


{% block title %} Register {% endblock title %}

{% block style %}
<style>
    .register-style {
        width:500px;
        height: auto;
    }
</style>
{% endblock style %}

{% block body %}
<div class="container mt-4 register-style">
    <h3>Register an account</h3>
    <form action="" method="post" novalidate>
        {% csrf_token %}
        {{user_form | crispy}}

        <input type="submit" value="Create Account" class="btn btn-success">
    </form>
</div>
{% endblock body %}
```

Cloud-Django/djqa/templates/register_done.html:

```py
{% extends 'base.html' %}

{% block title %} Register Done {% endblock title %}

{% block body %}
<div class="container mt-4">
    <p>Your account has been created, You can <a href="{% url 'login' %}">Login HERE</a>.</p>

</div>
{% endblock body %}
```

Cloud-Django/djqa/questions/urls.py:

```py
from django.urls import path
from .views import question_list, question_details, register

urlpatterns = [
    path('question/', question_list, name='question_list'),
    path('question/<slug:slug>/', question_details, name='question_details'),
    path('register/', register, name='register'),
]
```

Cloud-Django/djqa/templates/navbar.html:

```bsx
<a class="nav-link disabled">Welcome, {{request.user.username | title}}.</a>

<li class="nav-item">
  <a class="nav-link" href="{% url 'register' %}">Register</a>
</li>

```

```py
<style>
    .text-style {
        font-size: 30px !important;
        font-family: fantasy !important;
        color: brown !important;
        font-weight: bold !important;
    }
</style>

<nav class="navbar navbar-expand-lg bg-body-tertiary">
    <div class="container-fluid">
      <a class="navbar-brand text-style" href="#">Question Hub</a>
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav me-auto mb-2 mb-lg-0">
          <li class="nav-item">
            <a class="nav-link disabled">Welcome, {{request.user.username | title}}.</a>
          </li>
          <li class="nav-item">
            <a class="nav-link active" aria-current="page" href="#">Home</a>
          </li>

          <li class="nav-item">
            <a class="nav-link" href="#">Add Question</a>
          </li>

          <li class="nav-item dropdown">
            <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">
              Profile
            </a>
            <ul class="dropdown-menu">
              <li><a class="dropdown-item" href="#">Change Password</a></li>
              <li><a class="dropdown-item" href="#">Change Account</a></li>
              <li><a class="dropdown-item" href="#">Question & Answer</a></li>
              <li><hr class="dropdown-divider"></li>
              <li><a class="dropdown-item" href="{% url 'logout' %}">Logout</a></li>
            </ul>
          </li>

          <li class="nav-item">
            <a class="nav-link" href="{% url 'login' %}">Login</a>
          </li>

          <li class="nav-item">
            <a class="nav-link" href="{% url 'register' %}">Register</a>
          </li>
        </ul>
        <form class="d-flex" role="search">
          <input class="form-control me-2" type="search" placeholder="Search" aria-label="Search">
          <button class="btn btn-outline-success" type="submit">Search</button>
        </form>
      </div>
    </div>
  </nav>
```

![](https://user-images.githubusercontent.com/32337103/219070714-81862d60-505b-49f3-b4c6-197b254bd1ab.png)

![](https://user-images.githubusercontent.com/32337103/219070916-38b420bf-4df7-4184-9ca7-ce76c64e5e76.png)

![](https://user-images.githubusercontent.com/32337103/219070981-20e819ec-b57b-4002-a86e-15dd5c8a31bf.png)

![](https://user-images.githubusercontent.com/32337103/219071080-e21a3ed8-3acc-4337-a798-ae5c69914e98.png)

</details>

<details>
  <summary>49. Restricting Navbar from unauthorized Users </summary>

Cloud-Django/djqa/templates/navbar.html:

```bsx
{% if request.user.is_authenticated %}
---
{% else %}
---
{% endif %}
```

```py
<style>
    .text-style {
        font-size: 30px !important;
        font-family: fantasy !important;
        color: brown !important;
        font-weight: bold !important;
    }
</style>

<nav class="navbar navbar-expand-lg bg-body-tertiary">
    <div class="container-fluid">
      <a class="navbar-brand text-style" href="{% url 'question_list' %}">Question Hub</a>
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav me-auto mb-2 mb-lg-0">

          {% if request.user.is_authenticated %}

          <li class="nav-item mx-3">
            <a class="nav-link disabled">Welcome, {{request.user.username | title}}.</a>
          </li>

          <form class="d-flex" role="search">
            <input class="form-control me-2" type="search" placeholder="Search" aria-label="Search">
            <button class="btn btn-outline-success" type="submit">Search</button>
          </form>

          <li class="nav-item mx-3">
            <a class="nav-link active" aria-current="page" href="{% url 'question_list' %}">Home</a>
          </li>

          <li class="nav-item">
            <a class="nav-link" href="#">Add Question</a>
          </li>

          <li class="nav-item dropdown">
            <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">
              Profile
            </a>
            <ul class="dropdown-menu">
              <li><a class="dropdown-item" href="#">Change Password</a></li>
              <li><a class="dropdown-item" href="#">Change Account</a></li>
              <li><a class="dropdown-item" href="#">Question & Answer</a></li>
              <li><hr class="dropdown-divider"></li>
              <li><a class="dropdown-item" href="{% url 'logout' %}">Logout</a></li>
            </ul>
          </li>

          {% else %}

          <li class="nav-item">
            <a class="nav-link" href="{% url 'login' %}">Login</a>
          </li>

          <li class="nav-item">
            <a class="nav-link" href="{% url 'register' %}">Register</a>
          </li>

          {% endif %}
        </ul>

      </div>
    </div>
  </nav>
```

![](https://user-images.githubusercontent.com/32337103/219075501-966aa47a-a128-43a4-9160-ace0525f561a.png)

![](https://user-images.githubusercontent.com/32337103/219075571-ffda94c6-7f0f-4a37-9bc8-304cef526529.png)

![](https://user-images.githubusercontent.com/32337103/219075682-96850d19-e4ba-400c-8841-7ad2e10f6a68.png)

</details>

<details>
  <summary>50. Creating Questions </summary>

Cloud-Django/djqa/questions/forms.py:

```py
from django import forms
from django.contrib.auth import get_user_model
from .models import Question

User = get_user_model()

class UserRegistrationForm(forms.ModelForm):
    password = forms.CharField(label='Password', widget=forms.PasswordInput)
    password2 = forms.CharField(label='Confirm Password', widget=forms.PasswordInput)

    class Meta:
        model = User
        fields = ('username', 'first_name', 'email')

    def clean_password2(self):
        cd = self.cleaned_data
        if cd['password'] != cd['password2']:
            raise forms.ValidationError('Passwords don\'t match.')
        return cd['password2']

class QuestionRegistrationForm(forms.ModelForm):
    class Meta:
        model = Question
        fields = ('title', 'body',)
```

Cloud-Django/djqa/questions/models.py:

```py
from django.db import models
from django.conf import settings

# Create your models here.
class Question(models.Model):
    title= models.CharField(max_length=250)
    body = models.TextField()
    slug = models.SlugField(max_length=250, unique=True)
    author = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE,related_name='questions')
    created_at = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.title

class Answer(models.Model):
    description = models.TextField()
    question = models.ForeignKey(Question, on_delete=models.CASCADE, related_name='answers')
    author = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    created_at = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.author.username
```

Cloud-Django/djqa/questions/views.py:

```py
from django.shortcuts import render, get_object_or_404, redirect
from .models import Question
from .forms import UserRegistrationForm, QuestionRegistrationForm

# Create your views here.
def question_list(request):
    question_list = Question.objects.all().order_by('-created_at')
    return render(request, 'questionList.html', {'question_list': question_list})

def question_details(request, slug):
    question = get_object_or_404(Question, slug=slug )
    return render(request, 'questionDetails.html', {'question': question})

def register(request):
    if request.method == "POST":
        user_form = UserRegistrationForm(request.POST)

        if user_form.is_valid():
            new_user = user_form.save(commit=False)
            new_user.set_password(user_form.cleaned_data['password'])
            new_user.save()
            return render(request, 'register_done.html', {'user_form':user_form})
    else:
        user_form = UserRegistrationForm()
    return render(request, 'register.html', {'user_form':user_form})

def create_question(request):
    if request.method == "POST":
        question_form = QuestionRegistrationForm(request.POST)

        if question_form.is_valid():
            question = question_form.save(commit=False)
            question.author = request.user
            question = question_form.save()
            return redirect('question_list')
    else:
        question_form = QuestionRegistrationForm()
    return render(request, 'add_question.html', {'question_form': question_form})

```

Cloud-Django/djqa/templates/add_question.html:

```py
{% extends 'base.html' %}
{% load crispy_forms_tags %}


{% block title %} Add Question {% endblock title %}

{% block style %}
<style>
    .question-style {
        width:700px;
        height: auto;
    }
</style>
{% endblock style %}

{% block body %}
<div class="container mt-5 question-style">
    <h3>Add Question</h3>

    <form action="" method="post" novalidate>
        {% csrf_token %}
        {{question_form | crispy}}

        <input type="submit" value="Add Question" class="btn btn-success">
    </form>
</div>
{% endblock body %}
```

Cloud-Django/djqa/questions/urls.py:

```py
from django.urls import path
from .views import question_list, question_details, register, create_question

urlpatterns = [
    path('question/', question_list, name='question_list'),
    path('question/<slug:slug>/', question_details, name='question_details'),
    path('register/', register, name='register'),
    path('add/', create_question, name='create_question'),
]
```

Cloud-Django/djqa/templates/navbar.html:

```py
<style>
    .text-style {
        font-size: 30px !important;
        font-family: fantasy !important;
        color: brown !important;
        font-weight: bold !important;
    }
</style>

<nav class="navbar navbar-expand-lg bg-body-tertiary">
    <div class="container-fluid">
      <a class="navbar-brand text-style" href="{% url 'question_list' %}">Question Hub</a>
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav me-auto mb-2 mb-lg-0">

          {% if request.user.is_authenticated %}

          <li class="nav-item mx-3">
            <a class="nav-link disabled">Welcome, {{request.user.username | title}}.</a>
          </li>

          <form class="d-flex" role="search">
            <input class="form-control me-2" type="search" placeholder="Search" aria-label="Search">
            <button class="btn btn-outline-success" type="submit">Search</button>
          </form>

          <li class="nav-item mx-3">
            <a class="nav-link active" aria-current="page" href="{% url 'question_list' %}">Home</a>
          </li>

          <li class="nav-item">
            <a class="nav-link" href="{% url 'create_question' %}">Add Question</a>
          </li>

          <li class="nav-item dropdown">
            <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">
              Profile
            </a>
            <ul class="dropdown-menu">
              <li><a class="dropdown-item" href="#">Change Password</a></li>
              <li><a class="dropdown-item" href="#">Change Account</a></li>
              <li><a class="dropdown-item" href="#">Question & Answer</a></li>
              <li><hr class="dropdown-divider"></li>
              <li><a class="dropdown-item" href="{% url 'logout' %}">Logout</a></li>
            </ul>
          </li>

          {% else %}

          <li class="nav-item">
            <a class="nav-link" href="{% url 'login' %}">Login</a>
          </li>

          <li class="nav-item">
            <a class="nav-link" href="{% url 'register' %}">Register</a>
          </li>

          {% endif %}
        </ul>

      </div>
    </div>
  </nav>
```

![](https://user-images.githubusercontent.com/32337103/219086040-acc824f0-239a-4f15-add4-2ac761bf00e7.png)

![](https://user-images.githubusercontent.com/32337103/219086151-9f413ecd-2696-40f7-8430-34b5e14b1682.png)

![](https://user-images.githubusercontent.com/32337103/219086289-0718d64e-04a4-4a83-9531-9354b5b97205.png)

</details>

<details>
  <summary>51. Listing Answers </summary>

Cloud-Django/djqa/questions/views.py:

```bsx
def question_details(request, slug):
  question = get_object_or_404(Question, slug=slug)
  answer_list = Answer.objects.filter(question=question)
  return render(request, 'questionDetails.html', {'question': question, 'answer_list': answer_list})
```

```py
from django.shortcuts import render, get_object_or_404, redirect
from .models import Question, Answer
from .forms import UserRegistrationForm, QuestionRegistrationForm

# Create your views here.
def question_list(request):
    question_list = Question.objects.all().order_by('-created_at')
    return render(request, 'questionList.html', {'question_list': question_list})

def question_details(request, slug):
    question = get_object_or_404(Question, slug=slug)
    answer_list = Answer.objects.filter(question=question)
    return render(request, 'questionDetails.html', {'question': question, 'answer_list': answer_list})

def register(request):
    if request.method == "POST":
        user_form = UserRegistrationForm(request.POST)

        if user_form.is_valid():
            new_user = user_form.save(commit=False)
            new_user.set_password(user_form.cleaned_data['password'])
            new_user.save()
            return render(request, 'register_done.html', {'user_form':user_form})
    else:
        user_form = UserRegistrationForm()
    return render(request, 'register.html', {'user_form':user_form})

def create_question (request):
    if request.method == "POST":
        question_form = QuestionRegistrationForm(request.POST)

        if question_form.is_valid():
            question = question_form.save(commit=False)
            question.author = request.user
            question = question_form.save()
            return redirect('question_list')
    else:
        question_form = QuestionRegistrationForm()
    return render(request, 'add_question.html', {'question_form': question_form})

```

Cloud-Django/djqa/questions/models.py:

```py
from django.db import models
from django.conf import settings

# Create your models here.
class Question(models.Model):
    title= models.CharField(max_length=250)
    body = models.TextField()
    slug = models.SlugField(max_length=250, unique=True)
    author = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE,related_name='questions')
    created_at = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.title

class Answer(models.Model):
    description = models.TextField()
    question = models.ForeignKey(Question, on_delete=models.CASCADE, related_name='answers')
    author = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    created_at = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.author.username
```

Cloud-Django/djqa/templates/questionDetails.html:

```py
{% extends 'base.html' %}

{% block title %} Question Details {% endblock title %}

{% block body %}
<div class="container mt-3">
    <h1>{{question.title}}</h1>
    <p>{{question.body}}</p>
    <h6>
        Posted By: <i>{{question.author}} </i>
    </h6>
    <p>Published {{question.created_at}}</p>
    <hr>
</div>

<!-- Listing the answers -->
<div class="container">
    {% for answers in answer_list %}
        <div class="card mt-4 py-3 shadow">
            <div class="card-body">
                <p class="card-text">{{answers.description}}</p>
            </div>
        </div>
    {% endfor %}
</div>
{% endblock body %}
```

</details>

<details>
  <summary>52. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>53. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>54. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>55. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>56. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>57. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>58. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>59. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>60. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>61. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>62. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>63. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>64. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>65. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>66. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>67. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>68. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>69. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>70. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>71. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>72. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>73. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>74. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>75. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>76. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>77. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>78. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>79. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>80. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>81. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>82. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>83. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>84. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>85. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>86. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>87. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>88. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>89. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>90. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>91. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>92. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>93. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>94. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>95. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>96. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>97. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>98. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>99. </summary>

```py

```

```py

```

```py

```

```py

```

</details>

<details>
  <summary>100. </summary>

```py

```

```py

```

```py

```

```py

```

</details>
