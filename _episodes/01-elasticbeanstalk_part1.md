---
title: "AWS Elastic Beanstalk"
teaching: 0
exercises: 30
objectives:
- "Deploy a Django web framework on AWS Elastic Beanstalk"
- "Learn how to use a Python IDE to build your web app"
---
## Prerequisites
- [Anaconda](https://docs.continuum.io/anaconda/install) or working version of Python
- [Pycharm](https://www.jetbrains.com/pycharm/download/)
- [Pycharm AWS Elastic Beanstalk plugin](https://plugins.jetbrains.com/plugin/7388-aws-elastic-beanstalk-integration-for-web-languages)
- The web API code is available here: http://github.com/cloudmaven/web_api

## Deploying an Elastic Beanstalk App

Start PyCharm. 

Create a python environment for your app. You can use the "terminal" tab at the bottom of your PyCharm app or use the regular terminal. Here I am using conda to create my environment:

```
conda create -n env_name python
```

![](/cloud101_webframework/fig/02-elasticbeanstalk-0001.png)


You are now ready to create your new project. From the File menu, select New Project then Django. Fill in the location where you want to save your project (Here I am saving it to /Users/Amanda/PycharmProjects/cloud101demo). You can call your project whatever you want. Then choose your interepreter. Click the little "..." button on the right of the Interpreter text box, select "Add local" then find the virtual environment that you created earlier. If you created your environment using conda, it should be in your home directory under anaconda. For example, my virtual environment will be stored in ~/anaconda/envs/aws-env/. You will want to add the Python interpreter created in that virtual environment. Here, I am using Python 3.6.0. 

Under "More Settings", give your application a name. Here I'm calling mine "webapp". 

![](/cloud101_webframework/fig/02-elasticbeanstalk-0002.png)

You should now have a project with a folder structure that looks something like this:

![](/cloud101_webframework/fig/02-elasticbeanstalk-0003.png)

Create a folder named .ebextensions in the root of the project tree. This is the where AWS Beanstalk configuration files reside. Create a file named django.config inside the .ebextensions folder. Paste the following (remember to substitute beanstalkdemo for your project name):

~~~
option_settings:
  aws:elasticbeanstalk:container:python:
    WSGIPath: cloud101demo/wsgi.py
~~~

![](/cloud101_webframework/fig/02-elasticbeanstalk-0004.png)

Next, you need a requirements file to tell Beanstalk which dependencies to install when you deploy your Django app. Create a new file named requirements.txt in the root of the project. In your Terminal window (and under your python environment), you can use pip freeze to see what dependencies need to be installed. Select the dependencies and paste them into the requirements.txt file.

For example: 

~~~
appdirs==1.4.3
Django==1.10
packaging==16.8
pyparsing==2.2.0
six==1.10.0
~~~

You should now have a directory structure that looks like this:

![](/cloud101_webframework/fig/02-elasticbeanstalk-0005.png)

We can check the deployment of a generic Django website on your local machine. In the Tools dropdown menu, select Run manage.py Task

This will open up the manage.py console. Type migrate webapp then runserver:

![](/cloud101_webframework/fig/02-elasticbeanstalk-0006.png)

You will see a sample Django application being deployed on http://127.0.0.1:8000/
