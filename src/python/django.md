# Django

## Using Django Shell Plus inside a VS Code Notebook

### Preparing Django and the Python Virtual Env

Django Shell Plus is a nice way to explore your django project.

First on your project venv install the requisites:

```shell
pip install jupyterlab jupyter ipython django-extensions
```

Now add `django-extensions` to your `DJANGO_PROJECT/settings.py`:

```python
INSTALLED_APPS = [
    'django_extensions',
```

### Setting a fixed authentication 

It's boring to copy and paste the token everytime you start the notebook to the VSCode, so we have two ways:

_ps: disabling jupyter security does not work currently in vscode_

#### With a Password 
A nice trick is to set a password instead copying and pasting the `jupyter` token everytime you starts it, because jupyter changes it everytime you starts it.

Run this:

```shell
jupyter notebook password
```

To define a local password. This way VSCode will ask your password at least 4 times, but with a known value.

#### With a fixed token

Open `~/.jupyter/jupyter_notebook_config.json` with your favorite editor then paste:

```
{
  "NotebookApp": {
    "token": "MY_SECURE_TOKEN"
  }
}
```

The real trick is here: you need to use a "remote" jupyter notebook, because the notebook started by VS Code is in a state that I cannot load Django successfully, even settings paths, messing with `os.change_dir` and similar workarounds.

Press `CTRL+SHIFT+P` in VSCode and type `remote jupyter`. When asked for the server address put this:

```
http://localhost:8888/?token=MY_SECURE_TOKEN
```

Now reload VSCode. Run in a separate terminal the external Django Shell Plus notebook, inside your project dir:

```shell
cd DJANGO_PROJECT
```
Then
```
python manage.py shell_plus --notebook --no-browser
```

Create a new notebook file anywhere inside your project, like in `DJANGO_PROJECT/notebooks/test.ipynb`.

Before running your first django code, run any notebook cell, it will connect to your "remote" jupyter server and ask for the password, unless you are using the fixed token.

Now FINALLY change the kernel to Django Shell Plus on the VSCode GUI, upper right. 
Only then you can import your models and start exploring your django data.

On your first cell put this:

```python
import os
os.environ["DJANGO_ALLOW_ASYNC_UNSAFE"] = "true"
from myapp.models import Model1, Model2
Model1.objects.first()
```