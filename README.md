# To-Do List Web Application

This README provides an overview of a simple To-Do List web application built using Django, along with the code snippets for its key components.

## Table of Contents
1. [Project Description](#project-description)
2. [Installation](#installation)
3. [Key Components](#key-components)
   - [models.py](#modelspy)
   - [urls.py](#urlspy)
   - [views.py](#viewspy)
   - [main.html](#mainhtml)
   - [register.html](#registerhtml)
   - [login.html](#loginhtml)
   - [task_confirm_delete.html](#task_confirm_deletehtml)
   - [task_form.html](#task_formhtml)
   - [task_list.html](#task_listhtml)

## Project Description
This Django-based web application allows users to create and manage their to-do tasks. Users can register, log in, create new tasks, update task details, mark tasks as complete or incomplete, and delete tasks.

## Installation
1. Clone this repository to your local machine:

   ```
   git clone <repository-url>
   ```

2. Install the required dependencies:

   ```
   pip install -r requirements.txt
   ```

3. Run the Django development server:

   ```
   python manage.py runserver
   ```

4. Open your web browser and access the application at `http://localhost:8000`.

## Key Components

### models.py
The `models.py` file defines the data model for the to-do tasks. It includes a `Task` model that has fields such as `user`, `title`, `description`, `complete`, and `create`. The `Task` model is associated with the built-in `User` model for user authentication.

```python
from django.db import models
from django.contrib.auth.models import User

class Task(models.Model):
    user = models.ForeignKey(User, on_delete=models.CASCADE, null=True, blank=True)
    title = models.CharField(max_length=200)
    description = models.TextField(null=True, blank=True)
    complete = models.BooleanField(default=False)
    create = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.title

    class Meta:
        ordering = ["complete"]
```

### urls.py
The `urls.py` file defines the URL patterns and their corresponding view functions. It includes URL patterns for login, logout, registration, task list, task details, task creation, task update, and task deletion.

```python
from django.urls import path
from .views import (
    TaskList,
    TaskDetail,
    TaskCreate,
    TaskUpdate,
    TaskDelete,
    CustonLoginView,
    RegisterPage,
)
from django.contrib.auth.views import LogoutView

urlpatterns = [
    path("login/", CustonLoginView.as_view(), name="login"),
    path("logout/", LogoutView.as_view(next_page="login"), name="logout"),
    path("register/", RegisterPage.as_view(), name="register"),
    path("", TaskList.as_view(), name="tasks"),
    path("task/<int:pk>/", TaskDetail.as_view(), name="task"),
    path("task-create/", TaskCreate.as_view(), name="task-create"),
    path("task-update/<int:pk>/", TaskUpdate.as_view(), name="task-update"),
    path("task-delete/<int:pk>/", TaskDelete.as_view(), name="task-delete"),
]
```

### views.py
The `views.py` file contains the view functions for the application, including custom login and registration views, task list, task details, task creation, task update, and task deletion.

### main.html
The `main.html` file is the base HTML template for the application. It provides a common structure for all pages, including the header and styling.

### register.html
The `register.html` template is used for user registration. It includes a form for registering a new user and links to the login page for existing users.

### login.html
The `login.html` template is used for user login. It includes a form for users to log in and links to the registration page for new users.

### task_confirm_delete.html
The `task_confirm_delete.html` template is used to confirm task deletion. It displays a confirmation message and a button to delete the task.

### task_form.html
The `task_form.html` template is used for creating and updating tasks. It includes a form to input task details.

### task_list.html
The `task_list.html` template is used to display the list of tasks. It includes a search bar, task items, and links to perform actions like updating or deleting tasks.
