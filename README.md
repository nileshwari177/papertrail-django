PaperTrail Django App
PaperTrail is a simple Django-based web application that allows users to browse books, add them to a cart, and manage login/logout functionality.

 Table of Contents
Features

Project Structure

Technologies Used

Installation

Database and Migrations

Running the Project

Code Overview

Models

Views

Templates

URLs

Admin Panel

Future Enhancements


 Features
Book listing and browsing

Add to cart functionality

Simple login page for users

Admin dashboard to manage books

Django templating engine

SQLite as a lightweight backend database

Clear project and app structure following Django best practices

🗂 Project Structure
bash
Copy code
PaperTrail/
│
├── db.sqlite3
├── manage.py
│
├── pageturner/                  # App for core bookstore logic
│   ├── admin.py                 # Admin interface setup
│   ├── apps.py
│   ├── models.py                # Book model
│   ├── tests.py
│   ├── urls.py                  # App-level routing
│   ├── views.py                 # Book and cart views
│   ├── migrations/
│   ├── templates/
│   │   ├── booklist.html        # Book listing
│   │   ├── cart.html            # Cart view
│   │   └── login.html           # User login
│
└── papertrail/                  # Project-level configuration
    ├── asgi.py
    ├── settings.py              # Settings file
    ├── urls.py                  # Project URLs
    └── wsgi.py

    
 Technologies Used
Python 3.11

Django 3.x or higher

SQLite3

HTML/CSS

Django Templating Engine

Git (Version Control)

 Installation
Clone the repository

bash
Copy code
git clone https://github.com/your-username/PageTurner.git
cd PageTurner
Create and activate a virtual environment

bash
Copy code
python -m venv env
source env/bin/activate  # On Windows: env\Scripts\activate
Install dependencies

bash
Copy code
pip install django
Apply migrations

bash
Copy code
python manage.py makemigrations
python manage.py migrate
▶️ Running the Project
Start the development server:

bash
Copy code
python manage.py runserver
Visit: http://127.0.0.1:8000/

🗃 Database and Migrations
Uses SQLite by default (lightweight and built-in).

Migrations stored in pageturner/migrations/.

To create new migrations:

bash
Copy code
python manage.py makemigrations
python manage.py migrate
📄 Code Overview
📘 Models (pageturner/models.py)
python
Copy code
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
 Views (pageturner/views.py)
python
Copy code
from django.shortcuts import render
from .models import Book

def book_list(request):
    books = Book.objects.all()
    return render(request, 'booklist.html', {'books': books})

def cart_view(request):
    return render(request, 'cart.html')
Templates (pageturner/templates/)
booklist.html
html
Copy code
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
cart.html
html
Copy code
<h2>Your Cart</h2>
<p>No items in cart yet.</p>
login.html
html
Copy code
<form method="post">
    {% csrf_token %}
    <label>Username:</label><input type="text" name="username"><br>
    <label>Password:</label><input type="password" name="password"><br>
    <button type="submit">Login</button>
</form>
🌐 URLs
pageturner/urls.py
python
Copy code
from django.urls import path
from . import views

urlpatterns = [
    path('', views.book_list, name='book_list'),
    path('cart/', views.cart_view, name='cart'),
]
papertrail/urls.py
python
Copy code
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('pageturner.urls')),
]
🛒 Admin Panel
To manage books via Django Admin:

Register model in pageturner/admin.py:

python
Copy code
from django.contrib import admin
from .models import Book

admin.site.register(Book)
Create superuser:

bash
Copy code
python manage.py createsuperuser
Login at: http://127.0.0.1:8000/admin

 Future Enhancements
User registration and logout

Real cart functionality using sessions or cart model

Payment gateway integration (Stripe/Razorpay)

UI/UX improvements using Bootstrap or Tailwind

Book categories and search filters
