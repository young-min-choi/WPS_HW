# MEDIA ROOT AND URL

- Everyfile I uploaded was actually saved in a MEDIA ROOT folder. 
For me, it was __projects/django/instagram/django_app/media

- if settings.DEBUG: 
What does it mean? 
it means that the server is running for development purpose. 


```
in instagram/settings.py 

if settings.DEBUG:
    urlpatterns += [
        url(r'^static/(?P<path>.*)$', views.serve),
    ]
```

- What does the above mean? It means that when the server is running for development purpose, it will automatically add url for __STATIC MEDIA FILES__

> With this you can serve the static media from Django when DEBUG = True (when you run on local computer) but you can let your web server configuration serve static media when you go to production and DEBUG = False. 

- The following is important. 
	- In post/modes.py, photo = models.ImageField(__upload_to='post'__, blank=True)
	- my image file lives in __django_app/media/post/세기의_발견.png__
	- So if you want that file in the URL env, you can hit __:8000/media/post/세기의_발견.png__
	
- What is the role of the following? 
```
'django.template.context_processors.media',
``` 

> If this processor is enabled, every RequestContext will contain a variable MEDIA_URL, providing the value of the MEDIA_URL setting.

- The same goes with 'django.template.context_processors.static' 
>If this processor is enabled, every RequestContext will contain a variable STATIC_URL, providing the value of the STATIC_URL setting.

---
### STATIC 
- in settings.py, STATIC_DIR 

- staticfiles 
```
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
```

- {% load staticfiles %}

- 
``` 
<link rel="stylesheet" href="{{ STATIC_URL }}style.css" type="text/css" media="screen" />
```

- STATICFILES_DIRS = (
    "home/henk-jan/website/Template/Database")