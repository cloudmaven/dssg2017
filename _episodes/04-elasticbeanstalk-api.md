---
title: "AWS Elastic Beanstalk API"
teaching: 0
exercises: 10
questions:
- "How do I connect to my s3 bucket from my webapp"
objectives:
- "Build a simple API to display contents of an s3 bucket to a map"
keypoints:
- "Example of an API script"
- "Use s3 boto"
---
**What is a web framework?**
A framework helps building a website faster and easier by providing libraries and templates. Web frameworks help with streamlining these [overheads when creating a website](https://www.fullstackpython.com/web-frameworks.html):

1. URL routing
2. HTML, XML, JSON, and other output format templating
3. Database manipulation
4. Security against Cross-site request forgery (CSRF) and other attacks
5. Session storage and retrieval

Django is an example of a web framework. 

**Build your API**

We will create a new file in the app folder called api.py and stick this code in. 

Here we are creating the s3 connection using boto. When the month (e.g. Apr) and the variable (e.g. ET) are specified, this API will request the necessary file ('Mean_' + month + '_' + var + '.geojson') from your s3 bucket and dump the contents into "./test.geojson"

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
