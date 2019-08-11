# django-heroku
This repository contain a django application template hosted on Heroku cloud following latest builds !

link to application: [demo-django-heroku.herokuapp.com](https://demo-django-heroku.herokuapp.com/)

## Heroku Cloud

Heroku is a contained-based cloud platform for deploying, managing, and scalling applications. Even though there are other similar platforms, such as OpenShift by Red Hat, Windows Azure, Amazon Web Services, Google App Engine e.t.c, Heroku stands out because it is pretty basic and very easy to work with. It also comes with a free package for small apps.

## Deploying Django Application on heroku cloud

- Getting Started
  - Activate virtual-env for your django project and run ```pip install dj-database-url gunicorn whitenoise```
  - The dj-database-url will set up the database adapter in the project.
  - We will be running django application on Gunicorn in Heroku.
  - whitenoise will serve our static files on Heroku.

- Step 1: requirements.txt
  - **Make sure you are in the virtual env for the project**
  - Run the following command in the terminal to generate this file ```pip freeze >> requirements.txt```
  - This file tells Heroku the various libraries and packages that need to be installed to run your application.
  
- Step 2: database adapter
  - Heroku runs on Postgres, we will need to provide an adaptor for it.
  - Run the following command in the terminal ```pip install psycopg2```
  - Run ```pip freeze >> requirements.txt``` to update requirements.txt

- Step 3: Procfile
  - This file tells Heroku what to run.
  - Run the following command in the terminal to create this file ```touch Procfile```
  - Open it with a text editor or on your commandline using ```nano``` command and paste the following into it: ```web: gunicorn PROJECT_NAME.wsgi```
  - This will tell Heroku to run gunicorn with our ```.wsgi``` file.

- Step 4: update allowed_hosts and debug mode in settings.py
  - In settings.py set DEBUG to False
  - Change ALLOWED_HOSTS to ['*'] as we do not know our Heroku URL yet.

- Step 5: runtime.txt
  - In root directory create file ```runtime.txt```
  - This file will tell Heroku what version of Python to run our app with.
  - Inside the file, paste in ```python-3.7.3``` or whatever version you are using.
  - This repository contain code written in python-3.7.3

- Step 6: database configurations
  - In settings.py file replace the DATABASE objects with the following code and save
  - ``` 
        import dj_database_url
        DATABASES = {
            'default': dj_database_url.config(
            default='sqlite:////{0}'.format(os.path.join(BASE_DIR, 'db.sqlite3'))
            )
        }
    ```
  - This will allow us to keep using our local SQLite database while using Heroku’s database there.

- Step 7: Update Middleware for whitenoise v4.0
  - In settings.py, update MIDDLEWARE object for whitenoise v4.0
  - Add ```whitenoise.middleware.WhiteNoiseMiddleware``` just below ```django.middleware.security.SecurityMiddleware```

- Step 8: Setup Heroku
  - Install Heroku CLI
  - Create free Heroku account

- Step 9: Heroku Login
  - After you are done with above steps, run the following command ```heroku login``` on your terminal

- Step 10: Create Heroku Application
  - After successful login run following command to create a new app on heroku ```heroku create <your-app-name>```
  - You can also run ```heroku create``` and Heroku will give your application a random name

- Step 11: Add and Commit
  - Run following commands on your terminal ```git add .``` and,
  - ```git commit -m 'your-cimmit-message```

- Step 12: Push
  - Finally, you can deploy to Heroku by running ```git push heroku master``` on your terminal.

- Step 13: Start Web Process
  - You can tell Heroku to start this web process by running ```heroku ps:scale web=1``` on your terminal.

- Step 14: Run Migrations & create super-user
  - Since we created a new/empty database on Heroku, we will run migrations.
  - On your terminal, enter ```heroku run python manage.py migrate``` 
  - After that, create a django admin superuser ```heroku run python manage.py createsuperuser```.

- Step 16: Checkout
  - You can now visit your app in your browser by running ```heroku open``` command on the terminal.

That's it !

---

If you encounter any error after ```heroku open``` and message says to check logs simply do one thing, run the following command

```
heroku restart
```
This will give you a more precise description about what actually went wrong !
