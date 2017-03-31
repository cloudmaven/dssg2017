---
title: "AWS Elastic Beanstalk Leaflet"
teaching: 0
exercises: 15
questions:
- "Can we break up the lesson into multiple parts?"
objectives:
- "See that you can have more than one part for the lesson"
keypoints:
- "We can add parts to the lesson, by adding more markdown files"
- "You get the picture"
---
## ...Continued from [Part 1](/01-elasticbeanstalk.html)

**Build your API**

```python

#### Django modules
from django.http import  HttpResponse
from django.conf import settings

# #### External modules
import json
import boto
import boto.s3.connection


def simple_chart(request):

    month = request.GET.get('month', None)
    var = request.GET.get('var', None)

    filename = "./test.geojson"
    infile = 'Mean_' + month + '_' + var + '.geojson'

    # connect to the bucket
    conn = boto.connect_s3(settings.AWS_ACCESS_KEY_ID, settings.AWS_SECRET_ACCESS_KEY)
    bucket = conn.get_bucket(settings.BUCKET_NAME)

    key = bucket.get_key(infile)
    key.get_contents_to_filename(filename)

    geojsondata = open(filename).read()

    return HttpResponse(json.dumps(geojsondata))
```


Code is here: https://github.com/cloudmaven/cloud101demo_beanstalk/blob/master/app/api.py


**Build Leaflet**




## Django Web API

You will first need to install some packages and dependencies that your API will use. 

In the Terminal window in PyCharm you can install the packages you need:

~~~

pip install django-storages boto matplotlib django-leaflet 
pip freeze
~~~

Again, this will give you a list of of dependencies that you copy over to your requirements.txt file


~~~
#### Django modules
from django.shortcuts import render
from django.http import HttpRequest, HttpResponse, Http404, HttpResponseServerError, HttpResponseNotFound
from django.template import RequestContext
from django.views.decorators.cache import cache_control
from django.conf import settings
~~~

def simple_chart(request):
    assert isinstance(request, HttpRequest)
    
    month  = request.GET.get('month', None)
    var = request.GET.get('var', None)

    filename = "./test.geojson"
    infile = 'Mean_' + month + '_' + var + '.geojson'
    blob_service = BlobService(account_name='araldrift', account_key='xxx')
    blob_service.get_blob_to_path('flow', infile, filename)

    geojsondata = open(filename).read()

    return render(request, "map_api.html", {"geojsondata": geojsondata})
