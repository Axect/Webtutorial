# Django Guide

## Process

1. Activate Virtualenv
2. django-admin startproject nysite
3. mv mysite othername
4. Change settings.py 
> 1. TIME_ZONE = 'Asia/Seoul'
> 2. STATIC_ROOT = os.path.join(BASE\_DIR, 'static')
5. python manage.py migrate
6. python manage.py startapp someapp
7. Add app to settings.py
8. Edit models.py
> 1. from django.utils import timezone
> 2. Make class somename(models.Models)
> 3. models have belows :
>> * ForeignKey
>> * CharField
>> * TextField
>> * UrlField
>> * DateTimeField
9. python mange.py makemigrations someapp
10. python mange.py migrate someapp
11. Edit admin.py
12. admin.site.register( )
13. python manage.py createsuperuser
14. python manage.py runserver
15. Server : python manage.py collectstatic
16. Edit urls.py (mysite)
> ** Regex **
> * ^ : Start
> * $ : End
> * \d : Number
> * () : Save part of pattern
17. Edit urls.py (someapp)
> 1. from . import vies
> 2. Add urlpatterns 
18. Edit views.py