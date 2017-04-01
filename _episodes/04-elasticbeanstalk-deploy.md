---
title: "Leaflet Maps on AWS Elastic Beanstalk"
teaching: 0
exercises: 15
questions:
- "I've built my webapp, now what?"
objectives:
- "Learn to deploy a webapp to AWS Elastic Beanstalk"
keypoints:
- "Linking API calls and data visualization"
- "You get the picture"
---
## ...Continued from [Part 3](/03-elasticbeanstalk-leaflet.html)

**Deploying your app to Elastic Beanstalk using Pycharm**

Make sure you have the [Pycharm AWS Beanstalk Plugin](https://plugins.jetbrains.com/plugin/7274-aws-elastic-beanstalk-integration) installed. 

**Plugin Installation**

*Adapted from IntelliJ Intellij IDEA: AWS Elastic Beanstalk Integration from Viatra Documentation*

At the first you should install the plugin:
Go to “File”>”Settings”>”Plugins” window and press “Browse repositories”, repository browser window will open
Find “AWS Elastic Beanstalk Integration” plugin, make a right mouse button click and select “Download and Install” option in the context menu, press “Yes” in popup dialog
Close “Settings” window and restart IntelliJ IDEA to apply changes
Installed plugin should appear in your plugins list like on screenshot below



Cloud Credentials Setup
You need to create proper Application Server configuration:
Go to “Run/Debug Configurations” window
Press “Add new configuration” and select “AWS ElasticBeanstalk Deployment”
Press “...” button, clouds configuration window appears
Press “+” button and fill “Name”, “Access key” and “Secret key” fields. Security credendials can be obtained from AWS Account Security Credentials webpage.
Press “Test connection” button. All required libraries should be downloaded and you should see “Connection successful” message. If you see error messages - recheck correctness of your access and secret keys
If you want to deploy on particular AWS Region - select it in “Region” list
Press “OK” and save your settings





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
