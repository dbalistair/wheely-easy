# wheely-easy

Crash course on creating a Python wheel file
https://www.realpythonproject.com/how-to-create-a-wheel-file-for-your-python-package-and-import-it-in-another-project/
- python setup.py bdist_wheel --universal


How to bring libraries into the Databricks workspace (note, there are various ways to accomplish this)
https://docs.microsoft.com/en-us/azure/databricks/libraries/workspace-libraries

In this example, a wheel was built using PyCharm and the above instructions, then loaded to the Workspace using the UI:
- Workspace: Workspace : Create Library : Python Wheel : (drag and drop file)
- The library was loaded to dbfs:/FileStore/jars/b2b723e9_226d_4ec4_89f6_1ddadc3b42c5/alistair_easy_wheel-1.0-py2.py3-none-any.whl
- Using the instructions found here https://docs.microsoft.com/en-us/azure/databricks/libraries/notebooks-python-libraries#install-a-wheel-package-with-pip we then load the library into the cluster using %pip in a notebook (you could also load it using cluster configs)
- We have to adjust the path in the %pip install command, removing "dbfs:/" before "FileStore" and replacing with "/dbfs/"

Notebook Cmd 1:
- %pip install /dbfs/FileStore/jars/b2b723e9_226d_4ec4_89f6_1ddadc3b42c5/alistair_easy_wheel-1.0-py2.py3-none-any.whl

Notebook Cmd 2: 
- from test_wheel import my_functions
- print(my_functions.fn1())
- print(my_functions.fn2("hello"))

In Recap:
- Local Python work
- - Local Python files have functions created in my_functions.py inside test_wheel/ directory
- - Local Python files can use test_wheel.my_functions.fn1() and .fn2() just fine
- - We package up a wheel using the definition held inside setup.py which specifies using test_wheel's contents and calling the wheel alistair_easy_wheel
- Databricks work
- - To simplify this example we drag-and-drop the .whl file into the Create Library UI in the workspace, getting the stored path
- - We %pip install the .whl file, adjusting the path
- - We import my_functions from test_wheel and can use the functions in our notebook

# To Do:
In order to be able to get this wheel to be runnable from Databricks Jobs, we might need to deal with entry points, something this is not currently doing.
- https://databricks.com/blog/2022/02/14/deploy-production-pipelines-even-easier-with-python-wheel-tasks.html
- https://docs.microsoft.com/en-us/azure/databricks/jobs
- https://realpython.com/pypi-publish-python-package/#configuring-your-package
- https://amir.rachum.com/blog/2017/07/28/python-entry-points/
