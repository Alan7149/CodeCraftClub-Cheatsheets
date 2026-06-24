# 🎸 Django Cheatsheet

> Part of [CodeCraftClub-Cheatsheets](../README.md) · Django (Python web framework) quick reference.

---

## Setup & Commands

```bash
pip install django
django-admin startproject myproject .
python manage.py startapp blog
python manage.py runserver            # dev server :8000
python manage.py makemigrations
python manage.py migrate
python manage.py createsuperuser
python manage.py shell
python manage.py collectstatic
```

## Models

```python
# blog/models.py
from django.db import models

class Post(models.Model):
    title = models.CharField(max_length=200)
    body = models.TextField()
    created = models.DateTimeField(auto_now_add=True)
    published = models.BooleanField(default=False)
    author = models.ForeignKey(
        "auth.User", on_delete=models.CASCADE, related_name="posts"
    )

    class Meta:
        ordering = ["-created"]

    def __str__(self):
        return self.title
```

## ORM Queries

```python
Post.objects.all()
Post.objects.filter(published=True)
Post.objects.exclude(author=user)
Post.objects.get(id=1)                  # raises if not found
Post.objects.filter(title__icontains="django")
Post.objects.order_by("-created")[:10]
Post.objects.count()
Post.objects.create(title="Hi", body="...")
post.save(); post.delete()

# Lookups: __gt, __lt, __gte, __in, __startswith, __isnull
Post.objects.filter(created__gte=date)
Post.objects.filter(author__username="alan")   # FK traversal
```

## Views (Function-Based)

```python
# blog/views.py
from django.shortcuts import render, get_object_or_404, redirect
from .models import Post

def post_list(request):
    posts = Post.objects.filter(published=True)
    return render(request, "blog/list.html", {"posts": posts})

def post_detail(request, pk):
    post = get_object_or_404(Post, pk=pk)
    return render(request, "blog/detail.html", {"post": post})
```

## Views (Class-Based)

```python
from django.views.generic import ListView, DetailView

class PostList(ListView):
    model = Post
    template_name = "blog/list.html"
    context_object_name = "posts"

class PostDetail(DetailView):
    model = Post
```

## URLs

```python
# myproject/urls.py
from django.urls import path, include
urlpatterns = [
    path("admin/", admin.site.urls),
    path("blog/", include("blog.urls")),
]

# blog/urls.py
from . import views
urlpatterns = [
    path("", views.post_list, name="post_list"),
    path("<int:pk>/", views.post_detail, name="post_detail"),
]
```

## Templates

```django
{% extends "base.html" %}
{% block content %}
  {% for post in posts %}
    <h2>{{ post.title }}</h2>
    <p>{{ post.body|truncatewords:30 }}</p>
    <a href="{% url 'post_detail' post.pk %}">Read</a>
  {% empty %}
    <p>No posts.</p>
  {% endfor %}
  {% if user.is_authenticated %}Hi {{ user.username }}{% endif %}
{% endblock %}
```

## Forms

```python
from django import forms
class PostForm(forms.ModelForm):
    class Meta:
        model = Post
        fields = ["title", "body"]

# in view
form = PostForm(request.POST or None)
if form.is_valid():
    form.save()
```

## Admin

```python
# blog/admin.py
from django.contrib import admin
from .models import Post

@admin.register(Post)
class PostAdmin(admin.ModelAdmin):
    list_display = ["title", "author", "created"]
    list_filter = ["published"]
    search_fields = ["title"]
```

## Django REST Framework (API)

```python
# serializers.py
from rest_framework import serializers
class PostSerializer(serializers.ModelSerializer):
    class Meta:
        model = Post
        fields = "__all__"

# views.py
from rest_framework import viewsets
class PostViewSet(viewsets.ModelViewSet):
    queryset = Post.objects.all()
    serializer_class = PostSerializer

# urls.py
from rest_framework.routers import DefaultRouter
router = DefaultRouter()
router.register("posts", PostViewSet)
urlpatterns += router.urls
```

---

[🔝 Back to README](../README.md)
