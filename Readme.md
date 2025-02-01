# Setup OAuth 2 in Django

1. Install venv
````
python -m venv venv
````
2. Activate on mac
````
source venv/bin/activate
````
3. Create new project 
````
django-admin startproject djangoadmin
````
4. Open djangoadmin/settings.py and replace
````
DATABASES = {
    'default': {
        'ENGINE': 'mssql',
        'NAME': 'your_database_name',
        'USER': 'your_db_user',
        'PASSWORD': 'your_db_password',
        'HOST': 'your_db_host',  # Example: 'localhost' or 'your-server-ip'
        'PORT': '1433',  # Default SQL Server port
        'OPTIONS': {
            'driver': 'ODBC Driver 17 for SQL Server',  # Ensure this driver is installed
        },
    }
}
````
Now, create django app
````
python manage.py startapp myapp
````
5. Add Required Apps
````
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'rest_framework', # Add this
    'oauth2_provider',# Add this
    'myapp',  # Our custom Django app (we'll create this next)
]
````
6. Configure Django to use OAuth 2
````
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': (
        'oauth2_provider.contrib.rest_framework.OAuth2Authentication',
    ),
    'DEFAULT_PERMISSION_CLASSES': (
        'rest_framework.permissions.IsAuthenticated',
    ),
}
````
7. run migrations
````
python manage.py migrate
````
8. Create Super user
````
python manage.py createsuperuser
````
9. Start development server
````
python manage.py runserver
````
10. Open localhost:8080/admin to configure your app.
11. Login 
````
curl --location 'http://127.0.0.1:8000/oauth/token/' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'grant_type=password' \
--data-urlencode 'username=user' \
--data-urlencode 'password=Password' \
--data-urlencode 'client_id=oQdk6SP6Zpdvd2MI0iRkWo5IpB5RZAa0shkUHVcO' \
--data-urlencode 'client_secret=ljLCnZojZsS4Ww7V6Q6UcnFWXKI5tBqoM18OTrbGS4af9XmyMf6RadnKz85L2rhAqbSgrv9VJjsptwre5oPB672rCunAebfd1W84RqxYS4gduAY8mj0mrH3YY1C2uVKd'
````
