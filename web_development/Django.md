
# Django

## Table of Contents

1. Introduction to Django
2. Architecture and Request Lifecycle
3. Project Setup
4. Django Apps
5. URL Routing
6. Views
7. Templates
8. Models and ORM
9. Migrations
10. Forms and Validation
11. Authentication and Authorization
12. Django Admin
13. Class-Based Views
14. Django REST APIs
15. Middleware
16. Signals
17. Caching
18. Testing
19. Security Best Practices
20. Deployment
21. Recommended Project Structure
22. Example Application

---

# 1. Introduction to Django

Django is a high-level Python web framework that follows the **"batteries included"** philosophy.

Key features:

- ORM (Object Relational Mapper)
- Authentication system
- Admin interface
- Form handling
- Security protections
- Middleware support
- Caching
- Testing tools

Django is commonly used for:

- SaaS applications
- Internal business applications
- Content management systems
- E-commerce systems
- REST APIs

---

# 2. Architecture and Request Lifecycle

Django follows the **MTV pattern**:

- Model → Data layer
- Template → Presentation layer
- View → Business logic

Request flow:

```text
Browser
   ↓
URL Dispatcher
   ↓
View
   ↓
Model / Service Layer
   ↓
Template or JSON Response
   ↓
Browser
```

---

# 3. Project Setup

Install:

```bash
pip install django
```

Create project:

```bash
django-admin startproject config
```

Run server:

```bash
python manage.py runserver
```

Create app:

```bash
python manage.py startapp blog
```

Register app:

```python
INSTALLED_APPS = [
    ...
    "blog",
]
```

---

# 4. Django Apps

Apps are reusable modules.

Example:

```text
blog/
├── models.py
├── views.py
├── urls.py
├── admin.py
├── tests.py
└── migrations/
```

A project may contain many apps.

Examples:

- users
- payments
- blog
- inventory

---

# 5. URL Routing

App URLs:

```python
from django.urls import path
from .views import post_list

urlpatterns = [
    path("", post_list, name="post-list"),
]
```

Project URLs:

```python
from django.urls import include, path

urlpatterns = [
    path("blog/", include("blog.urls")),
]
```

URL names allow reverse lookups:

```python
reverse("post-list")
```

---

# 6. Views

Function-based view:

```python
from django.http import HttpResponse

def home(request):
    return HttpResponse("Hello Django")
```

JSON response:

```python
from django.http import JsonResponse

def api_status(request):
    return JsonResponse({"status": "ok"})
```

---

# 7. Templates

View:

```python
from django.shortcuts import render

def home(request):
    return render(request, "home.html")
```

Template:

```html
<h1>{{ title }}</h1>

{% for post in posts %}
    <p>{{ post.title }}</p>
{% endfor %}
```

Template inheritance:

```html
{% extends "base.html" %}
```

---

# 8. Models and ORM

Model:

```python
from django.db import models

class Post(models.Model):
    title = models.CharField(max_length=200)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.title
```

Queries:

```python
Post.objects.all()

Post.objects.filter(title__icontains="django")

Post.objects.get(id=1)

Post.objects.create(
    title="Hello",
    content="World"
)
```

---

# 9. Migrations

Create migration:

```bash
python manage.py makemigrations
```

Apply migration:

```bash
python manage.py migrate
```

Show SQL:

```bash
python manage.py sqlmigrate blog 0001
```

---

# 10. Forms and Validation

Form:

```python
from django import forms

class ContactForm(forms.Form):
    name = forms.CharField()
    email = forms.EmailField()
```

Validation:

```python
def clean_name(self):
    value = self.cleaned_data["name"]

    if len(value) < 3:
        raise forms.ValidationError("Too short")

    return value
```

---

# 11. Authentication and Authorization

Create user:

```python
from django.contrib.auth.models import User

User.objects.create_user(
    username="alice",
    password="secret"
)
```

Protect view:

```python
from django.contrib.auth.decorators import login_required

@login_required
def dashboard(request):
    ...
```

Permission check:

```python
request.user.has_perm(
    "blog.change_post"
)
```

---

# 12. Django Admin

Register model:

```python
from django.contrib import admin
from .models import Post

admin.site.register(Post)
```

Create superuser:

```bash
python manage.py createsuperuser
```

Admin customisation:

```python
@admin.register(Post)
class PostAdmin(admin.ModelAdmin):
    list_display = (
        "id",
        "title",
        "created_at"
    )
```

---

# 13. Class-Based Views

List view:

```python
from django.views.generic import ListView
from .models import Post

class PostListView(ListView):
    model = Post
    template_name = "posts/list.html"
```

Detail view:

```python
from django.views.generic import DetailView

class PostDetailView(DetailView):
    model = Post
```

---

# 14. Django REST APIs

Install:

```bash
pip install djangorestframework
```

Serializer:

```python
from rest_framework import serializers

class PostSerializer(serializers.ModelSerializer):
    class Meta:
        model = Post
        fields = "__all__"
```

ViewSet:

```python
from rest_framework.viewsets import ModelViewSet

class PostViewSet(ModelViewSet):
    queryset = Post.objects.all()
    serializer_class = PostSerializer
```

Router:

```python
from rest_framework.routers import DefaultRouter

router = DefaultRouter()
router.register("posts", PostViewSet)
```

---

# 15. Middleware

Middleware executes around requests.

```python
class RequestLogMiddleware:

    def __init__(self, get_response):
        self.get_response = get_response

    def __call__(self, request):
        response = self.get_response(request)
        return response
```

Register:

```python
MIDDLEWARE = [
    ...
    "core.middleware.RequestLogMiddleware",
]
```

---

# 16. Signals

Signal example:

```python
from django.db.models.signals import post_save
from django.dispatch import receiver

@receiver(post_save, sender=Post)
def notify(sender, instance, created, **kwargs):
    if created:
        print("Post created")
```

Use signals carefully to avoid hidden business logic.

---

# 17. Caching

Local memory cache:

```python
CACHES = {
    "default": {
        "BACKEND":
        "django.core.cache.backends.locmem.LocMemCache"
    }
}
```

Usage:

```python
from django.core.cache import cache

cache.set("key", "value", 60)

value = cache.get("key")
```

Production typically uses Redis.

---

# 18. Testing

Model test:

```python
from django.test import TestCase

class PostTests(TestCase):

    def test_create_post(self):
        post = Post.objects.create(
            title="Test",
            content="Content"
        )

        self.assertEqual(post.title, "Test")
```

Client test:

```python
response = self.client.get("/")

self.assertEqual(
    response.status_code,
    200
)
```

---

# 19. Security Best Practices

Django includes:

- CSRF protection
- XSS protection
- SQL injection protection
- Clickjacking protection

Production recommendations:

```python
DEBUG = False

SECURE_SSL_REDIRECT = True

SESSION_COOKIE_SECURE = True

CSRF_COOKIE_SECURE = True
```

Store secrets in environment variables.

---

# 20. Deployment

Common stack:

```text
Nginx
   ↓
Gunicorn/Uvicorn
   ↓
Django
   ↓
PostgreSQL
```

Collect static files:

```bash
python manage.py collectstatic
```

Run migrations:

```bash
python manage.py migrate
```

---

# 21. Recommended Project Structure

```text
project/
├── apps/
│   ├── users/
│   ├── blog/
│   └── payments/
├── config/
├── templates/
├── static/
├── media/
├── tests/
└── manage.py
```

For larger projects:

- Service layer
- Repository pattern (optional)
- Domain-driven design concepts
- Dependency injection where useful

---

# 22. Example Blog Application

Model:

```python
class Post(models.Model):
    title = models.CharField(max_length=200)
    body = models.TextField()
```

View:

```python
def post_list(request):
    posts = Post.objects.all()

    return render(
        request,
        "posts/list.html",
        {"posts": posts}
    )
```

Template:

```html
{% for post in posts %}
    <h2>{{ post.title }}</h2>
{% endfor %}
```

URL:

```python
path("", post_list)
```

This demonstrates Django's complete flow:

1. Request arrives
2. URL resolves view
3. View queries model
4. Template renders HTML
5. Response returns to browser

---

# What to Learn Next

1. Django REST Framework
2. PostgreSQL optimization
3. Celery background jobs
4. Redis caching
5. Docker
6. Nginx
7. CI/CD with GitHub Actions
8. Automated testing
9. Async Django
10. Multi-tenant SaaS architecture

