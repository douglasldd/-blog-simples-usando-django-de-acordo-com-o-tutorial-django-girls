mkdir meu-blog
cd meu-blog
python -m venv myvenv
pip install django

django-admin startproject mysite .
python manage.py startapp blog

from django.db import models
from django.utils import timezone

class Post(models.Model):
    author = models.ForeignKey('auth.User', on_delete=models.CASCADE)
    title = models.CharField(max_length=200)
    text = models.TextField()
    created_date = models.DateTimeField(default=timezone.now)
    published_date = models.DateTimeField(blank=True, null=True)

    def publish(self):
        self.published_date = timezone.now()
        self.save()

    def __str__(self):
        return self.title

python manage.py makemigrations blog
python manage.py migrate
python manage.py createsuperuser

from django.shortcuts import render
from .models import Post

def post_list(request):
    posts = Post.objects.filter(published_date__lte=timezone.now()).order_by('published_date')
    return render(request, 'blog/post_list.html', {'posts': posts})


# blog/urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('', views.post_list, name='post_list'),
]

<html>
    <head><title>Meu Blog Douglas</title></head>
    <body>
        <h1><a href="/">Douglas Blog</a></h1>
        {% for post in posts %}
            <div>
                <p>Publicado: {{ post.published_date }}</p>
                <h2><a href="">{{ post.title }}</a></h2>
                <p>{{ post.text|linebreaksbr }}</p>
            </div>
        {% endfor %}
    </body>
</html>

python manage.py runserver

127.0.0.1



