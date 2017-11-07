# Django Guide

## Process

1. Activate Virtualenv
2. django-admin startproject mysite
3. Change settings.py 
> 1. TIME_ZONE = 'Asia/Seoul'
> 2. STATIC_ROOT = os.path.join(BASE\_DIR, 'static')
4. python manage.py migrate
5. python manage.py startapp someapp
6. Add app to settings.py
7. Edit models.py
> 1. from django.utils import timezone
> 2. Make class somename(models.Model)
> 3. models have belows :
>> * ForeignKey
>> * CharField
>> * TextField
>> * UrlField
>> * DateTimeField
> ```python
> from django.db import models
> from django.utils import timezone
> import ast
>
> # Create your models here.
>
> class Hi(models.Model):
>     author = models.ForeignKey('auth.User')
>     title = models.CharField(max_length=10)
>     body = models.TextField(null=True, blank=True)
>     created_date = models.DateTimeField(
>         default=timezone.now
>     )
>     published_date = models.DateTimeField(
>         null=True, blank=True
>     )
>
>     def publish(self):
>         self.published_date = timezone.now
>         self.save
>
>     def __str__(self):
>         return self.title
> ```

8. python manage.py makemigrations someapp
9. python manage.py migrate someapp
10. Edit admin.py
> 1. (In VS Code, install djaneiro)
> 2. from .models import *
> 3. Typing adminview and tab!
> ```python
> from django.contrib import admin
> from .models import *
>
>class HiAdmin(admin.ModelAdmin):
>    '''
>        Admin View for Hi
>    '''
>    list_display = ('title', 'body', 'author', 'published_date')
>    list_filter = ('title', 'author')
>    search_fields = ('title', 'author')
>    list_per_page = 25
>
>admin.site.register(Hi, HiAdmin)
>```
11. python manage.py createsuperuser
12. python manage.py runserver
13. Server : python manage.py collectstatic
14. Edit urls.py (mysite)
> ** Regex **
> * ^ : Start
> * $ : End
> * \d : Number
> * () : Save part of pattern
> ```python
> from django.conf.urls import include, url
> from django.contrib import admin
>
> admin.site.site_title = "Yonsei HEP-COSMO Admin Page"
> admin.site.site_header = "Yonsei HEP-COSMO Admin Page"
> 
> urlpatterns = [
>     url(r'^admin/', admin.site.urls),
>     url(r'', include('HEP_COSMO.urls')),
> ]
> ```
16. Edit urls.py (someapp)
> 1. from . import views
> 2. Add urlpatterns 
> ```python
> from django.conf.urls import url
> from . import views
>
> urlpatterns = [
>     url(r'^$', views.home, name='index'),
>     url(r'^index', views.home, name='index'),
>     url(r'^members', views.members, name='members'),
>     url(r'^publish', views.publish, name='publish'),
>     url(r'^research', views.research, name='research'),
>     url(r'^calendar', views.calendar, name='calendar'),
>     url(r'^finedust', views.finedust, name='finedust'),
>     url(r'^link', views.link, name='link'),
>     url(r'^arxiv', views.arxiv, name='arxiv'),
>     url(r'contact', views.contact, name='contact'),
> ]
> ```

17. Edit views.py (render : request -> html)
> ```python
> from django.shortcuts import render
> from .models import *
>
> # Create your views here.
>
> def home(request):
>     return render(request, 'HEP/index.html', {})
> 
> def members(request):
>     persons = People.objects.order_by('index')
>     temp = []
>     for person in persons:
>         temp.append(person.position)
>     positions = []
>     [positions.append(position) for position in temp if position not in positions]
>     return render(request, 'HEP/members.html', {'persons': persons, 'positions': positions})
> ```

18. mkdir templates, mkdir templates/app, touch html
19. Edit html ({% block content %})
> ```html
> {% load staticfiles %}
> <html lang="en">
> <head>
>     <meta charset="utf-8">
>     <meta name="viewport" content="width=device-width,  initial-scale=1.0">
>     <meta name="description" content="Yonsei HEP-COSMO Web Page">
>     <meta name="author" content="Axect">
>
>     <title>Yonsei HEP-COSMO</title>
> 
>     <!-- Bootstrap core CSS -->
>     <link href="{% static 'css/bootstrap.css' %}" rel="stylesheet">
>     <link href="{% static 'css/image-effects.css' %}" rel="stylesheet">
>     <link href="{% static 'css/custom-styles.css' %}" rel="stylesheet">
>     <link href="{% static 'css/font-awesome.css' %}" rel="stylesheet">
>     <link href="{% static 'css/font-awesome-ie7.css' %}" rel="stylesheet">
>     <!-- Favicon -->
>     <link rel="shortcut icon" href="{% static 'favicon.ico' %}" />
> </head>
> ```

> ```html
> <div class="container">
>     <div class="row">
>         <div class="col-md-12 col-sm-12">
>             <div class="blist">
>                 {% for diffyear in yearlist %}
>                     <div class="ruler"></div>
>                     <ul>
>                         <li><a href="#">{{ diffyear }}</a></li>
>                     </ul>
>                     <div class="dlist">
>                         <ul>
>                             {% for article in articles %}
>                             {% if article.year == diffyear %}
>                                 <li>
>                                     {{ article.people }}, &#12300;<span><i>{{ article.title }}</i></span>&#12301; 
>                                     {% if article.journal !=  None %}
>                                         <a href="{{ article.hyperlink }}" target="_blank">{{ article.journal }}</a>
>                                     {% endif %}
>                                     <a href="{{ article.arxivlink }}" target="_blank">{{ article.arxiv }}</a>
>                                 </li>
>                             {% endif %}
>                             {% endfor %}
>                         </ul>
>                     </div>
>                 {% endfor %}
>                 <br />
>                 <div class="ruler"></div>
>                  <p>
>                     You can find the full list from InspireHEP [<a href="https://goo.gl/AS6UKo" target="_blank">here</a>]
>                 </p>
>             </div>
>         </div>
>     </div>
> </div>
> ```

20. mkdir static, mkdir css, touch css