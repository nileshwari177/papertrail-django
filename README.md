#  PaperTrail – A Simple Django Bookstore

**PaperTrail** is a Django-based web application that allows users to browse books, add them to a cart, and manage login/logout functionality.

---

##  Project Structure

```
PaperTrail/
│
├── db.sqlite3
├── manage.py
│
├── pageturner/                # App for core bookstore logic
│   ├── admin.py               # Admin interface setup
│   ├── apps.py
│   ├── models.py              # Book model
│   ├── tests.py
│   ├── urls.py                # App-level routing
│   ├── views.py               # Book and cart views
│   ├── migrations/            # Migration files
│   └── templates/             # HTML templates
│       ├── booklist.html      # Book listing
│       ├── cart.html          # Cart view
│       └── login.html         # User login
│
└── papertrail/                # Project-level configuration
    ├── asgi.py
    ├── settings.py            # Settings file
    ├── urls.py                # Project URLs
    └── wsgi.py
```

---

##  Technologies Used

- Python 3.11  
- Django 3.x or higher  
- SQLite3  
- HTML/CSS  
- Django Templating Engine  
- Git (Version Control)

---

##  Installation

1. **Clone the repository**:

```
git clone https://github.com/your-username/PageTurner.git
cd PageTurner
```

2. **Create and activate a virtual environment**:

```
python -m venv env
source env/bin/activate         # On Windows: env\Scripts\activate
```

3. **Install dependencies**:

```
pip install django
```

4. **Apply migrations**:

```
python manage.py makemigrations
python manage.py migrate
```

5. **Run the development server**:

```
python manage.py runserver
```

 Visit: [http://127.0.0.1:8000/](http://127.0.0.1:8000/)

---

##  Code Overview

###  Models – `pageturner/models.py`

```python
from django.db import models

class Book(models.Model):
    title = models.CharField(max_length=200)
    author = models.CharField(max_length=100)
    description = models.TextField()
    price = models.FloatField()
    image_url = models.URLField(max_length=200)
    available = models.BooleanField(default=True)

    def __str__(self):
        return self.title
```
### `views.py`
```python
from django.shortcuts import render
from .models import Book

def book_list(request):
    books = Book.objects.all()
    return render(request, 'booklist.html', {'books': books})

def cart_view(request):
    return render(request, 'cart.html')


```
### `urls.py (App-level)`
```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.book_list, name='book_list'),
    path('cart/', views.cart_view, name='cart'),
]

```
### ` templates/booklist.html`
```html
{% for book in books %}
    <div class="book-card">
        <h3>{{ book.title }}</h3>
        <p>{{ book.author }}</p>
        <p>₹{{ book.price }}</p>
        <img src="{{ book.image_url }}" alt="Book image" width="150">
        {% if book.available %}
            <p>In Stock</p>
        {% else %}
            <p>Out of Stock</p>
        {% endif %}
    </div>
{% endfor %}


```
### ` templates/cart.html`
```html
<h2>Your Cart</h2>
<p>No items in cart yet.</p>


```
### `templates/login.html`
```html
<form method="post">
    {% csrf_token %}
    <label>Username:</label><input type="text" name="username"><br>
    <label>Password:</label><input type="password" name="password"><br>
    <button type="submit">Login</button>
</form>


```
### `Register model in admin.py`
```python
from django.contrib import admin
from .models import Book

admin.site.register(Book)



```

---

##  Migrations and Database

- **Database**: Uses built-in SQLite3 (lightweight and easy for development).
- **Migrations**: Stored in `pageturner/migrations/`.

To create new migrations:

```
python manage.py makemigrations
python manage.py migrate
```


---

##  Features

- Book browsing with detailed listing.
- Cart system to add/remove books.
- User login functionality.
- Clean and simple UI using HTML/CSS.
- Backend logic using Django’s MVC pattern.

---


---

