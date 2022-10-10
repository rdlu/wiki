# Jupyter Notebook Tips

## Notebook folder using remote VSCode

When you use a docker container with Jupyter Notebook and VSCode Remote Connect feature to develop your notebooks, you can have difficulties to load local python libraries inside you folder.

You need to set the correct working directory with this:

```python
# Set the correct folder for Remote VSCode
import os

try:
    # '.' if the path is to current folder
    os.chdir(os.path.join(os.getcwd(), './notebooks'))
    # print(os.getcwd())
except:
    pass
os.getcwd()
```

Then you will have easier time to `import` your libs.
