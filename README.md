# django-commands (For Git Bash in Windows)
### create python venv
 1. Make sure to have python installed, and update pip version
      ```bash
      py -m pip install --upgrade pip
      ```
 2. Create a directory and in the directory create a python venv:
      ```bash
      mkdir <directory_name>
      ```
      ```bash
      cd <directory_name>
      ```
      ```bash
      py -m venv <env_name>
      ```
  3[1].  To activate venv 
      ```bash
      source ./<env_name>/Scripts/activate
      ``` 
  3[2].  To deactivate venv 
      ```bash
      deactivate
      ``` 
### Install django library
 1. In the directory, install django with pip
      ```bash
      pip install django
      ``` 
 2. To check installed django version
      ```bash
      python -m django version
      ```
      or
      ```bash
      pip list
      ``` 
### Create a django project
 - In the directory, while venv has activated, run:
   ```bash
     django-admin startproject <project_name>
   ```

### Create a django app
 - In the directory, while venv has activated, run:
 - ```bash
     cd <project_name>
   ```
   ```bash
     python manage.py startapp <app_name>
   ```

### Launch development server
 1. In the directory while venv has activated, run:
   ```bash
     cd <project_name> 
   ```
   ```bash
      python manage.py runserver 
   ```
 2. If the bash responded with:
 ```bash
 Watching for file changes with StatReloader
 ```
 - This mean server successfully launched. You can now open localhost. By default, the development server will be accessible at http://127.0.0.1:8000/ or http://localhost:8000/.
3. To stop development server, just hit ctrl+c in bash.


### Creating a view and URL configuration
1. make sure to have django installed
2. in the views.py
   ```python
     from django.shortcuts import render
     from django.http import HttpResponse
     def home(request):
         return HttpResponse("Hello World!")
   ```
3. in the App-level urls.py(If not exists please create one.)
    ```python
     from django.urls import path
     from . import views
     urlpatterns = [
             path('', views.home, name="home"),
     ] 
   ```
4. in the Project-level urls.py
   ```python
     from django.contrib import admin
     from django.urls import path, include
     
     urlpatterns = [
         path('admin/', admin.site.urls),
         path('', include('myapp.urls')),
     ] 
   ```
5. in the setting.py
   - add "'myapp.apps.MyappConfig'," to the INSTALLED_APPS list.
   ```bash
     INSTALLED_APPS = [        
         'django.contrib.admin',
         'django.contrib.auth',
         'django.contrib.contenttypes',
         'django.contrib.sessions',
         'django.contrib.messages',
         'django.contrib.staticfiles',
         'myapp.apps.MyappConfig', 
    ]
   ```
### Create URL path with views HttpResponse
1. make sure to have django installed
2. in the views.py
   ```python
     from django.shortcuts import render
     from http.client import HTTPResponse
     from django.http import HttpResponse
     
     def drinks(request, drink_name):
         drink = {
             'mocha' : 'type of coffee',
             'tea' : 'type of hot beverage',
             'lemonade': 'type of refreshment'
         }
         choice_of_drink = drink[drink_name]
         return HttpResponse(f"<h2>{drink_name}</h2> " + choice_of_drink)

   ```
3. in the App-level urls.py (If not exists please create one)
   ```python
     from django.urls import path
     from . import views
     
     urlpatterns = [
         path('drinks/<str:drink_name>', views.drinks, name="drink_name"), 
     ]
   ```
### App-level urls.py namespace
- adding "app_name = <app_name>" to App-level urls.py
  ```python
    #demoapp/urls.py
    from django.urls import path  
    from . import views    
    app_name='demoapp' 
    urlpatterns = [  
        path('', views.index, name='index'),      
    ] 
  ```
