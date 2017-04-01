---
title: "Leaflet Maps on AWS Elastic Beanstalk"
teaching: 0
exercises: 20
questions:
- "How do I build a Leaflet map?"
objectives:
- "Build a Leaflet map that displays the contents of test.geojson"
keypoints:
- "Linking API calls and data visualization"
- "You get the picture"
---
## ...Continued from [Part 2](/02-elasticbeanstalk-api.html)

**Build your map page **

First create an HTML file called map_api.html in your templates folder

Right click on Templates > New > HTML file > map_api.html

The Leaflet code is here: https://github.com/cloudmaven/cloud101demo_beanstalk/blob/master/templates/map_api.html

We will walk through this code quickly together. 


**Tying all the pieces together**
The last two things we need to do is to put references to our API and map page in our code. 

The first is editing the views.py file which contains the views python function. This is where we specify what happens when there is a web request. 

In views.py under the app folder, we will add this code:

```python
from django.template.response import TemplateResponse

# Create your views here.
def home(request):
    """Renders the home page."""
    return TemplateResponse(request, 'app/index.html', {
            'title':'Home Page'
        })

def map_api(request):
    """Renders the map page."""
    month = request.GET.get('month', None)
    var = request.GET.get('var', None)

    return TemplateResponse(
        request,
        'map_api.html',{
            'title':'MAP_API',
            'message':'Generate the spatial plot for average values',
            'month': month,
            'var': var
        }
    )
```

Next, we have to set up the URL configuration. This python code provides the mapping for URLs. In the urls.py file under cloud101demo (or name of your project) subfolder, we will add this snippet: 

```python 
from django.conf.urls import url
from django.contrib import admin
from app import views
from app import api

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^$', views.home, name='home'),
    url(r'^map_api', views.map_api, name='map_api'),
    url(r'^api/simple_chart', api.simple_chart),
]
```
