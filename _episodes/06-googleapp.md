---
title: "Django on Google App Engine"
teaching: 0
exercises: 25
questions:
- "How do I deploy a Django Web Framework on Google App Engine?"
objectives:
- "Reuse our app that we built in PyCharm"
- "Deploy to Google App Engine using the command line gcloud"
keypoints:
- "A summary of everything so far"
---

## Prerequisites
- gcloud SDK (Follow the installation [here](https://cloud.google.com/sdk/downloads))


**HOUSTON WE HAVE A PROBLEM**
You are not allowed to change the quota for your project is you are using the $300 free trial. This app will deploy but the map won't display because the geojson file download exceeds the maximum allowable transfer rate :-( We will walk through these steps regardless because you can see how the code works on a local deployment. 

You will use the project you created for AWS Beanstalk to deploy on the Google App Engine Flexible Environment - this is the equivalent to AWS Beanstalk. To start a new Google App Engine in PyCharm from scratch, you can use this reference: https://www.jetbrains.com/help/pycharm/2016.3/google-app-engine.html

## Create new project in Google Console 

Open the Cloud Platform Console (http://console.cloud.google.com)

In the drop-down menu at the top, select Create a project.

Click Show advanced options. Under App Engine location, select a United States location.

Give your project a name.

Make a note of the project ID, which might be different from the project name (This usually contains a long list of numbers). The project ID is used in commands and in configurations.


## Modifying your code
Since you are using boto to connect to Google Cloud Storage, you will also need to use the gcs_oauth2_boto_plugin which currently only works with Python 2.7. To do this, you will first need to create a Python 2.7 environment and assign that as the interpreter for our project. In your terminal window on your local machine, do:

```python 
conda create -n gcppy27 python=2.7
```

Go to "Pycharm" > "Preferences" > "Project Interpreter" and select the Python 2.7 environment you just created. 

Next, you will install the gcs_oauth2_boto_plugin and gunicorn. In the terminal window in pycharm, 

```python
pip install gcs_oauth2_boto_plugin gunicorn 
```

Next, we will add an app.yaml file to your root directory. This is the configuration file that tells Google App Engine how to deploy your project. In the app.yaml file, add this snippet:

```python
runtime: python
env: flex
entrypoint: gunicorn -b :$PORT cloud101demo.wsgi

runtime_config:
  python_version: 2
```

In the settings.py file, add your Google Cloud credentials: 

```
GS_ACCESS_KEY_ID = ''
GS_SECRET_ACCESS_KEY = ''
GS_PROJECT_ID = '876941683495'
GS_BUCKET_NAME = 'cloud101demo'
GOOGLE_STORAGE = 'gs'
```

In api.py, you can comment out the s3 bucket and use the following code snippet instead:

```python
gcs_oauth2_boto_plugin.SetFallbackClientIdAndSecret(settings.GS_ACCESS_KEY_ID, settings.GS_SECRET_ACCESS_KEY)
src_uri = boto.storage_uri(settings.GS_BUCKET_NAME + '/' + infile, settings.GOOGLE_STORAGE)

key = src_uri.get_key(infile)
key.get_contents_to_filename(filename)
```

Again, we will create a new requirements.txt file. In the Terminal, 

```python
pip freeze > requirements.txt
```

You can then preview your app locally by going again to Run > Django server. 

## Deploying your Django app
You will need to install the gcloud SDK. 

Next, using your Terminal or bash shell, navigate to you Pycharm project folder. You will add the as a git repo to your Google app. 

```bash
$ git add .
$ git commit -am "message"
$ git remote add google https://source.developers.google.com/p/cloud101demo-163421/r/cloud101demo
```

Then configure gcloud, push your git commit and deploy your app:

```bash
$ gcloud auth login
$ which gcloud
$ source GCLOUD_DIRECTORY_FROM_ABOVE/path.bash.inc 
$ gcloud config set project cloud101demo-163421
$ git push --all google
$ gcloud app deploy
```
Your app should be available at: (yourappid.appspot.com)
