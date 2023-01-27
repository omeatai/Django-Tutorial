# Django-Tutorial

Django Tutorial by Ifeanyi Omeata

## Tutorial

---

### [1-DJANGO TUTORIAL - W3SCHOOLS](#)

<details>
  <summary>1. Introduction </summary>

Django follows the MVT design pattern (Model View Template).

- Model - The data you want to present, usually data from a database.
- View - A request handler that returns the relevant template and content - based on the request from the user.
- Template - A text file (like an HTML file) containing the layout of the web page, with logic on how to display the data.

Model <br>

The model provides data from the database.

- In Django, the data is delivered as an Object Relational Mapping (ORM), which is a technique designed to make it easier to work with databases.

- The most common way to extract data from a database is SQL. One problem with SQL is that you have to have a pretty good understanding of the database structure to be able to work with it.

- Django, with ORM, makes it easier to communicate with the database, without having to write complex SQL statements.

- The models are usually located in a file called models.py.

View

- A view is a function or method that takes http requests as arguments, imports the relevant model(s), and finds out what data to send to the template, and returns the final result.

- The views are usually located in a file called views.py.

Template

- A template is a file where you describe how the result should be represented.

- Templates are often .html files, with HTML code describing the layout of a web page, but it can also be in other file formats to present other results, but we will concentrate on .html files.

- Django uses standard HTML to describe the layout, but uses Django tags to add logic.

- The templates of an application is located in a folder named templates.

```html
<h1>My Homepage</h1>

<p>My name is {{ firstname }}.</p>
```

URLs

- Django also provides a way to navigate around the different pages in a website.

- When a user requests a URL, Django decides which view it will send it to.

- This is done in a file called urls.py.

</details>

<details>
  <summary>2. Install Python, pip and Venv </summary>

Download Python:

```py
https://www.python.org/
```

Check for Python Version:

```py
python --version
```

```py
# Python 3.9.12
```

Install pip:

```py
https://pypi.org/project/pip/
```

Check pip version:

```py
pip --version
```

```py
# pip 21.2.4
```

Upgrade pip:

```py
python -m pip install --upgrade pip
```

Install venv and activate the virtual environment:

```py
python -m venv venv-w3django

source venv-w3django/bin/activate
```

</details>

<details>
  <summary>3. Install Django </summary>

```py
python -m pip install Django
```

Check Django version:

```py
django-admin --version
```

```py
# 4.1.5
```

</details>

<details>
  <summary>4. Create Django Project </summary>

```py
django-admin startproject my_tennis_club
django-admin startproject my_tennis_club .
```

run django project -

my_tennis_club/

```py
python manage.py runserver
```

```py
# System check identified no issues (0 silenced).

# You have 18 unapplied migration(s). Your project may not work properly until you apply the migrations for app(s): admin, auth, contenttypes, sessions.
# Run 'python manage.py migrate' to apply them.
# January 26, 2023 - 19:26:37
# Django version 4.1.5, using settings 'my_tennis_club.settings'
# Starting development server at http://127.0.0.1:8000/
# Quit the server with CONTROL-C.
```

127.0.0.1:8000:

![001](https://user-images.githubusercontent.com/32337103/214931443-5581766b-920d-4ddb-ac99-c649421fa3d1.png)

</details>

<details>
  <summary>5. Create Django App </summary>

my_tennis_club/

```py
python manage.py startapp members
```

</details>

<details>
  <summary>6. Create Django View - return HttpResponse </summary>

members/views.py:

```py
from django.shortcuts import render
from django.http import HttpResponse

def members(request):
    return HttpResponse("Hello world!")
```

</details>

<details>
  <summary>7. Create urls.py routes </summary>

my_tennis_club/members/urls.py:

```py
from django.urls import path
from . import views

urlpatterns = [
    path('members/', views.members, name='members'),
]
```

my_tennis_club/my_tennis_club/urls.py:

```py
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('', include('members.urls')),
    path('admin/', admin.site.urls),
]
```

/my_tennis_club:

```py
python manage.py runserver
```

127.0.0.1:8000/members:

![002](https://user-images.githubusercontent.com/32337103/214957810-f91a2461-1028-4d23-bcd4-9ef782e741e4.png)

</details>

<details>
  <summary>8. Create and load HTML Template </summary>

my_tennis_club/members/templates/myfirst.html:

```html
<!DOCTYPE html>
<html>
  <body>
    <h1>Hello World!</h1>
    <p>Welcome to my first Django project!</p>
  </body>
</html>
```

my_tennis_club/members/views.py:

```py
from django.http import HttpResponse
from django.template import loader

def members(request):
  template = loader.get_template('myfirst.html')
  return HttpResponse(template.render())
```

</details>

<details>
  <summary>9. Register App in Settings </summary>

my_tennis_club/my_tennis_club/settings.py:

```py
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'members'
]
```

Run Migrations:

```py
# python manage.py makemigrations
python manage.py migrate
```

/my_tennis_club

```py
python manage.py runserver
```

127.0.0.1:8000/members:

![004](https://user-images.githubusercontent.com/32337103/214960898-da8f5076-7b10-4112-a4cd-f9d12589fc5d.png)

</details>

<details>
  <summary>10. Create Member Model </summary>

my_tennis_club/members/models.py:

```py
from django.db import models

class Member(models.Model):
  firstname = models.CharField(max_length=255)
  lastname = models.CharField(max_length=255)
```

my_tennis_club/

```py
python manage.py makemigrations members
```

```py
python manage.py migrate
```

View SQL migrate:

```py
python manage.py sqlmigrate <model> <migration number>
python manage.py sqlmigrate members 0001
```

```py
# BEGIN;
# --
# -- Create model Member
# --
# CREATE TABLE "members_member" ("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, "firstname" varchar(255) NOT NULL, "lastname" varchar(255) NOT NULL);
# COMMIT;
```

</details>

<details>
  <summary>11. Get and Insert Data </summary>

Enter Python Shell

```py
python manage.py shell
```

```py
# Python 3.9.13 (v3.9.13:6de2ca5339, May 17 2022, 11:37:23)
# [Clang 13.0.0 (clang-1300.0.29.30)] on darwin
# Type "help", "copyright", "credits" or "license" for more information.
# (InteractiveConsole)
# >>>
```

GET Data from Members Table (Model):

```py
>>> from members.models import Member
>>> Member.objects.all()
```

```py
# <QuerySet []>
```

POST/Add a single Data record to Members Table (Model):

```py
>>> member = Member(firstname='Emil', lastname='Refsnes')
>>> member.save()
```

View added record in Model:

```py
>>> Member.objects.all()
```

```py
# <QuerySet [<Member: Member object (1)>]>
```

```py
>>> Member.objects.all().values()
```

```py
# <QuerySet [{'id': 1, 'firstname': 'Emil', 'lastname': 'Refsnes'}]>
```

To add Multiple Records in the Model:

```py
>>> member1 = Member(firstname='Tobias', lastname='Refsnes')
>>> member2 = Member(firstname='Linus', lastname='Refsnes')
>>> member3 = Member(firstname='Lene', lastname='Refsnes')
>>> member4 = Member(firstname='Stale', lastname='Refsnes')
>>> member5 = Member(firstname='Jane', lastname='Doe')
>>> members_list = [member1, member2, member3, member4, member5]
>>> for x in members_list:
>>>   x.save()
```

View added records in Model:

```py
>>> Member.objects.all().values()
```

```py
# <QuerySet [{'id': 1, 'firstname': 'Emil', 'lastname': 'Refsnes'},
# {'id': 2, 'firstname': 'Tobias', 'lastname': 'Refsnes'},
# {'id': 3, 'firstname': 'Linus', 'lastname': 'Refsnes'},
# {'id': 4, 'firstname': 'Lene', 'lastname': 'Refsnes'},
# {'id': 5, 'firstname': 'Stale', 'lastname': 'Refsnes'},
# {'id': 6, 'firstname': 'Jane', 'lastname': 'Doe'}]>
```

</details>

<details>
  <summary>12. Update Data Records </summary>

Get the record for member at index 4, which is "Stale Refsnes":

```py
>>> from members.models import Member
>>> x = Member.objects.all()[4]
>>> x.firstname
```

```py
# 'Stale'
```

Now change the value of this record:

```py
>>> x.firstname = "Stalikken"
>>> x.save()
```

```py
>>> Member.objects.all().values()
```

```py
# <QuerySet [{'id': 1, 'firstname': 'Emil', 'lastname': 'Refsnes'},
# {'id': 2, 'firstname': 'Tobias', 'lastname': 'Refsnes'},
# {'id': 3, 'firstname': 'Linus', 'lastname': 'Refsnes'},
# {'id': 4, 'firstname': 'Lene', 'lastname': 'Refsnes'},
# {'id': 5, 'firstname': 'Stalikken', 'lastname': 'Refsnes'},
# {'id': 6, 'firstname': 'Jane', 'lastname': 'Doe'}]>
```

</details>

<details>
  <summary>13. Delete Data Records </summary>

Get the record you want to delete:

```py
>>> from members.models import Member
>>> x = Member.objects.all()[5]
>>> x.firstname
```

```py
# 'Jane'
```

Now delete the record:

```py
>>> x.delete()
```

```py
# (1, {'members.Member': 1})
```

```py
>>> Member.objects.all().values()
```

```py
# <QuerySet [{'id': 1, 'firstname': 'Emil', 'lastname': 'Refsnes'},
# {'id': 2, 'firstname': 'Tobias', 'lastname': 'Refsnes'},
# {'id': 3, 'firstname': 'Linus', 'lastname': 'Refsnes'},
# {'id': 4, 'firstname': 'Lene', 'lastname': 'Refsnes'},
# {'id': 5, 'firstname': 'Stalikken', 'lastname': 'Refsnes'}]>
```

```py
>>> exit()
```

</details>

<details>
  <summary>14. Add Fields (Columns) to Model </summary>

my_tennis_club/members/models.py:

```py
from django.db import models

class Member(models.Model):
  firstname = models.CharField(max_length=255)
  lastname = models.CharField(max_length=255)
  phone = models.IntegerField()
  joined_date = models.DateField()
```

Make Migrations:

```py
python manage.py makemigrations members
```

```py
# You are trying to add a non-nullable field 'joined_date' to members without a default; we can't do that (the database needs something to populate existing rows).
# Please select a fix:
#  1) Provide a one-off default now (will be set on all existing rows with a null value for this column)
#  2) Quit, and let me add a default in models.py
# Select an option:
```

```bs
Select option 2: allow NULL values for the two new fields.
```

my_tennis_club/members/models.py:

```py
from django.db import models

class Member(models.Model):
  firstname = models.CharField(max_length=255)
  lastname = models.CharField(max_length=255)
  phone = models.IntegerField(null=True)
  joined_date = models.DateField(null=True)
```

Make Migrations:

```py
python manage.py makemigrations members
```

```py
# Migrations for 'members':
#   members/migrations/0002_member_joined_date_member_phone.py
#     - Add field joined_date to member
#     - Add field phone to member
```

Run the migrate command:

```py
python manage.py migrate
```

```py
# Operations to perform:
#   Apply all migrations: admin, auth, contenttypes, members, sessions
# Running migrations:
#   Applying members.0002_member_joined_date_member_phone... OK
```

</details>

<details>
  <summary>15. Insert Data into updated Model </summary>

```py
python manage.py shell
```

```py
# Python 3.9.2 (tags/v3.9.2:1a79785, Feb 19 2021, 13:44:55) [MSC v.1928 64 bit (AMD64)] on win32
# Type "help", "copyright", "credits" or "license" for more information.
# (InteractiveConsole)
# >>>
```

```py
>>> from members.models import Member
>>> x = Member.objects.all()[0]
>>> x.phone = 5551234
>>> x.joined_date = '2022-01-05'
>>> x.save()
```

```py
>>> Member.objects.all().values()
```

```py
# <QuerySet [
# {'id': 1, 'firstname': 'Emil', 'lastname': 'Refsnes', 'phone': 5551234, 'joined_date': datetime.date(2022, 1, 5)},
# {'id': 2, 'firstname': 'Tobias', 'lastname': 'Refsnes', 'phone': None, 'joined_date': None},
# {'id': 3, 'firstname': 'Linus', 'lastname': 'Refsnes', 'phone': None, 'joined_date': None},
# {'id': 4, 'firstname': 'Lene', 'lastname': 'Refsnes', 'phone': None, 'joined_date': None},
# {'id': 5, 'firstname': 'Stalikken', 'lastname': 'Refsnes', 'phone': None, 'joined_date': None}]>
```

</details>

<details>
  <summary>16. sample </summary>

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
  <summary>17. sample </summary>

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
  <summary>18. sample </summary>

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
  <summary>19. sample </summary>

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
  <summary>20. sample </summary>

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
  <summary>21. sample </summary>

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
  <summary>22. sample </summary>

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
  <summary>23. sample </summary>

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
  <summary>24. sample </summary>

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
  <summary>25. sample </summary>

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
  <summary>26. sample </summary>

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
  <summary>27. sample </summary>

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
  <summary>28. sample </summary>

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
  <summary>29. sample </summary>

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
  <summary>30. sample </summary>

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
  <summary>31. sample </summary>

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
  <summary>32. sample </summary>

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
  <summary>33. sample </summary>

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
  <summary>34. sample </summary>

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
  <summary>35. sample </summary>

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
  <summary>36. sample </summary>

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
  <summary>37. sample </summary>

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
  <summary>38. sample </summary>

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
  <summary>39. sample </summary>

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
  <summary>40. sample </summary>

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
  <summary>41. sample </summary>

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
  <summary>42. sample </summary>

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
  <summary>43. sample </summary>

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
  <summary>44. sample </summary>

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
  <summary>45. sample </summary>

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
  <summary>46. sample </summary>

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
  <summary>47. sample </summary>

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
  <summary>48. sample </summary>

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
  <summary>49. sample </summary>

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
  <summary>50. sample </summary>

```py

```

```py

```

```py

```

```py

```

</details>
