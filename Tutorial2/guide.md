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
8. python manage.py makemigrations someapp
9. python manage.py migrate someapp
10. Edit admin.py
11. admin.site.register( )
12. python manage.py createsuperuser
13. python manage.py runserver
14. Server : python manage.py collectstatic
15. Edit urls.py (mysite)
> ** Regex **
> * ^ : Start
> * $ : End
> * \d : Number
> * () : Save part of pattern
16. Edit urls.py (someapp)
> 1. from . import views
> 2. Add urlpatterns 
17. Edit views.py (render : request -> html)
18. mkdir templates, mkdir templates/app, touch html
19. Edit html ({% block content %})
20. mkdir static, mkdir css, touch css