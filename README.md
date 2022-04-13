# sphinx-tutorial
### dummy files to illustrate how to use sphinx and readthedocs.io

---

This is a simple tutorial showing how to use sphinx to document your code.
In this folder there is some very simple code, and we want to create some nice
looking documentation.

Before you start let us create a virtual environment where we will install all our stuff

```
   pip install virtualenv
   virtualenv --python python3 venv
   source venv/bin/activate
```

Let us also install the dummy package

```
   python setup.py install
```

# Install sphinx

First thing we need to do in order to use sphinx is to install it

```
   pip install sphinx
```
# Create the doc directory

Next we will create a the directory where we will put the documentation

```
   mkdir doc
   cd doc
```

Now we run the `sphinx-quickstart` which is the first step in generating documentation.

```
    (venv) user@linux:~/sphinx-tutorial/doc$ sphinx-quickstart 
Welcome to the Sphinx 4.5.0 quickstart utility.

Please enter values for the following settings (just press Enter to
accept a default value, if one is given in brackets).

Selected root path: .

You have two options for placing the build directory for Sphinx output.
Either, you use a directory "_build" within the root path, or you separate
"source" and "build" directories within the root path.
> Separate source and build directories (y/n) [n]: n

The project name will occur in several places in the built documentation.
> Project name: Mypackage
> Author name(s): Tyrone Lee
> Project release []: 1

If the documents are to be written in a language other than English,
you can select a language here by its language code. Sphinx will then
translate text that it generates into that language.

For a list of supported codes, see
https://www.sphinx-doc.org/en/master/usage/configuration.html#confval-language.
> Project language [en]:

Creating file /home/projects/sphinx-tutorial/docs/conf.py.
Creating file /home/projects/sphinx-tutorial/docs/index.rst.
Creating file /home/projects/sphinx-tutorial/docs/Makefile.
Creating file /home/projects/sphinx-tutorial/docs/make.bat.

Finished: An initial directory structure has been created.

You should now populate your master file /home/daedalus/projects/sphinx-tutorial/docs/index.rst and create other documentation
source files. Use the Makefile to build the docs, like so:
   make builder
where "builder" is one of the supported builders, e.g. html, latex or linkcheck.
```
You can look at your documentation by running


```
   make html
   python -m http.server
```

Then open a webbrowser and go to `localhost:8000`, and navigate to `build/html`.

# Creating the API documentation


Now we will make the documentation for our python package

```
   sphinx-apidoc -o source/ ../myproject/
```

If you run `make html` now you will get a warning saying "Unexpected section title",
and this is because I have documented the code using the numpy style, which
is not default. Open `source/conf.py` and add the extenstion `'sphinx.ext.napoleon'` to the list
called `extensions`. The HTML theme and other options can also be selected, see 
https://www.sphinx-doc.org/en/master/usage/quickstart.html
Scroll down and set `html_theme = 'sphinx_rtd_theme'`, and run `pip install sphinx-rtd-theme`.

Now you can run  ````make html````.



# Adding the README to the docs

Let us add this README file to the docs. Copy this README file to the source directory

```
   cp ../README.rst source/.
```   
Next, open `source/index.rst` and put in the following command in there

```

   .. toctree::
      README
      myproject
      modules

```   

# Create pdf documentation

This step will require latex but can easily be built with

```

   make latexpdf
   open build/latex/Mypackage.pdf
```

# readthedocs.io

readthedocs' preferred way are .readthedocs.yaml, which needs to live in the root folder. Following https://docs.readthedocs.io/en/stable/config-file/v2.html, this is how the file now looks like:

```
version: 2

sphinx:
  configuration: docs/conf.py

python:
  version: 3.8
  install:
    - requirements: requirements.txt
    - requirements: docs/requirements.txt
```
