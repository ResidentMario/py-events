##Structure of the application
**Procfile** - This command is run whenever the application is restarted on Bluemix. It should be linked to your application's webservice: in this case, `app.py`. It is represented in the form `web: <command>` where `<command>`..

**requirements.txt** - Contains the external python packages that are required by the application. These will be downloaded from the [python package index](https://pypi.python.org/pypi/) and installed via the python package installer (pip) during the buildpack's compile stage when you execute the cf push command. To ensure compatability deploy these same packages with `pip -r requirements.txt` on your development machine as well.

**runtime.txt** - Controls which python runtime to use. In this case we want to use 3.4.3. 

**README.md** - This readme.

**app.py** - The python web app, implemented in Python [Flask](http://flask.pocoo.org/). The routes are defined in the application using the @app.route() calls. This application has a / route and a /myapp route defined. The application deployed to Bluemix needs to listen to the port defined by the VCAP_APP_PORT environment variable as seen here:
```python
port = os.getenv('VCAP_APP_PORT', '5000')
if __name__ == "__main__":
    app.run(host='0.0.0.0', port=int(port))
```

This is the port given to your application so that http requests can be routed to it. If the property is not defined then it falls back to port 5000 allowing you to run this sample appliction locally.

##Development sketch

Bluemix does not provide an event scheduling service, so the email capacity will have to be done via a administrative portal in the web app in the long run, and probably out of the local environment in the short run. This is done via the flask-email plugin. Emails ought to be delivered on a weekly basis.

There is no feasible way to database cultural events. Instead this application will rely on a mock-up "content curator", someone who will use an admintool to plug events into the database.

For testing purposes I will define a 100 (?)-event starter dataset manually. This dataset will not be metricated by date.

Flushing out the IBM Watson methods could be tough, given that the API is not being finished. However, only the initial email need be delivered in real time. The rest can be run using tokens, since they will run al at once weekly.

##Development schedule

Embed Flask emailing into application.

Create the 100-event manual list. Spin the existing access methods against it to see if I need or want two-iter.

...