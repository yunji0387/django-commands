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
1. using instance namespace (If use this please skip step 2 - 4)
   - in Project-level urls.py
     ```python
       #in demoproject/urls.py 
       urlpatterns=[ 
           # ... 
           path('demo/', include('demoapp.urls', namespace='demoapp')), 
           # ... 
       ] 
     ```
3. adding "app_name = <app_name>" to App-level urls.py
  ```python
    #demoapp/urls.py
    from django.urls import path  
    from . import views    
    app_name='demoapp' 
    urlpatterns = [  
        path('', views.index, name='index'),      
    ] 
  ```
3. we can also use namespace with more than one app
  ```python
    #newapp/urls.py 
   from django.urls import path 
   from . import views 
   app_name='newapp' 
   urlpatterns = [ 
       path('', views.index, name='index'), 
   ] 
  ```
4. In the Project-level urls.py
  ```python
    from django.contrib import admin 
    from django.urls import path, include 
    
    urlpatterns = [ 
        path('admin/', admin.site.urls), 
        path('demo/', include('demoapp.urls')), 
        path('newdemo/', include('newapp.urls')), 
    ] 
  ```
5. To check the url path/route in django shell
  ```bash
    python manage.py shell 
  ```
  ```bash
    >>> reverse('demoapp:index') 
    '/demo/' 
  ```
  ```bash
    >>> reverse('newapp:index') 
    '/newdemo/' 
  ```
6. In views.py
   ```python
    from django.http import HttpResponsePermanentRedirect 
    from django.urls import reverse 
      
    def myview(request): 
        .... 
        return HttpResponsePermanentRedirect(reverse('demoapp:login'))
   ```
7. Using namespace in the url tag
 - without using namespace
     ```html
      <form action="/demoapp/login" method="post"> 
      #form attributes 
      <input type='submit'> 
      </form> 
     ```
     ```html
      <form action="{% url 'login' %}" method="post"> 
      #form attributes 
      <input type='submit'> 
      </form> 
     ```
 - using namespace
     ```html
      <form action="{% url 'demoapp:login' %}" method="post"> 
      #form attributes 
      <input type='submit'> 
      </form> 
     ```
### views.py Http Error Handling 
- Basic error handling (in views.py)
   ```python
   from django.http import HttpResponse, HttpResponseNotFound 
   def my_view(request): 
       # ... 
       if condition==True: 
           return HttpResponseNotFound('<h1>Page not found</h1>') 
       else: 
           return HttpResponse('<h1>Page was found</h1>') 
   ```
   or
  ```python
   from django.http import HttpResponse 
   def my_view(request): 
       # ... 
       if condition==True: 
           return HttpResponse('<h1>Page not found</h1>', status_code='404') 
       else: 
           return HttpResponse('<h1>Page was found</h1>') 
   ```
- Raise error handling (in views.py)
   ```python
   from django.http import Http404, HttpResponse 
   from .models import Product 
   
   def detail(request, id): 
       try: 
           p = Product.objects.get(pk=id) 
       except Product.DoesNotExist: 
           raise Http404("Product does not exist") 
       return HttpResponse("Product Found") 
   ```
  - Custom page error handling (in views.py)
    1. First in settings.py set DEBUG to FALSE and add '*' to the ALLOWED_HOSTS to include all possible hosts.
       ```python
       #settings.py      
       # SECURITY WARNING: don't run with debug turned on in production!        
       DEBUG=FALSE
       ALLOWED_HOSTS = ['*']
       ```
    2. ...
  - Exception Classes
    - ObjectDoesNotExist: All the exceptions of the DoesNotExist are inherited from this base exception.
    - EmptyResultSet: This exception is raised if a query does not return any result.
    - FieldDoesNotExist: This exception is raised when the requested field does not exist.
      ```python
      try: 
         field = model._meta.get_field(field_name) 
      except FieldDoesNotExist: 
         return HttpResponse("Field Does not exist") 
      ```
    - MultipleObjectsReturned: When you expect a certain query to return only a single object, however multiple objects are returned. This is when you need to raise this exception.
    - PermissionDenied: This exception is raised when a user does not have permission to perform the action requested.
      ```python
      def myview(request): 
          if not request.user.has_perm('myapp.view_mymodel'): 
              raise PermissionDenied() 
          return HttpResponse() 
      ```
    - ViewDoesNotExist: This exception is raised by django.urls when a requested view does not exist, possibly because of incorrect mapping defined in the URLconf.
    - Django’s Form API
      ```python
      def myview(request):   
          if request.method == "POST":   
              form = MyForm(request.POST)   
              if form.is_valid():   
                  #process the form data 
              else:   
                      return HttpResponse("Form submitted with invalid data") 
      ```
