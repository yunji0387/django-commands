# django-commands
### create python venv in git bash for Windows
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
### Install django in git bash for Window
 1. In the project directory, install django with pip
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
