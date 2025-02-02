.. _devguide_setup:

Getting Set Up
==============

The Panel library is a complex project which provides a wide range
of data interfaces and an extensible set of plotting backends, which
means the development and testing process involves a wide set of
libraries.

.. contents::
    :local:
    :depth: 2

.. dev_guide_preliminaries:

Preliminaries
-------------

Git
~~~

The Panel source code is stored in a `Git`_ source control repository.
The first step to working on Panel is to install Git on to your system.
There are different ways to do this depending on whether, you are using
Windows, OSX, or Linux.

To install Git on any platform, refer to the `Installing Git`_ section of
the `Pro Git Book`_.

Conda
~~~~~

Developing Panel requires a wide range of packages that are not
easily and quickly available using pip. To make this more manageable,
core developers rely heavily on the `conda package manager`_ for the
free `Anaconda`_ Python distribution. However, ``conda`` can also
install non-Python package dependencies, which helps streamline Panel
development greatly. It is *strongly* recommended that anyone
developing Panel also use ``conda``, and the remainder of the
instructions will assume that ``conda`` is available.

To install Conda on any platform, see the `Download conda`_ section of the
`conda documentation`_.

Cloning the Repository
----------------------

The source code for the Panel project is hosted on GitHub_. To clone the
source repository, issue the following command:

.. code-block:: sh

    git clone https://github.com/holoviz/panel.git

This will create a ``panel`` directory at your file system
location. This ``panel`` directory is referred to as the *source
checkout* for the remainder of this document.


Create a development environment
--------------------------------

Since Panel interfaces with a large range of different libraries the
full test suite requires a wide range of dependencies. To make it
easier to install and run different parts of the test suite across
different platforms Panel uses a library called pyctdev to make things
more consistent and general. To start with `cd` into the panel
directory and then set up conda using the following commands:

.. code-block:: sh

    cd panel
    conda install -c pyviz "pyctdev>0.5.0"
    doit ecosystem_setup

Once pyctdev is available and you are in the cloned panel repository you can
set up an environment with:

.. code-block:: sh

    doit env_create -c pyviz/label/dev -c conda-forge --name=panel_dev --python=3.7

Specify the desired Python version, currently Panel officially
supports Python 3.7, 3.8 and 3.9. Once the environment has been
created you can activate it with:

.. code-block:: sh

    conda activate panel_dev


.. _devguide_python_setup:

Install Panel in editable mode
------------------------------

To perform an editable install of Panel, including all the
dependencies required to run the full unit test suite, run the
following:

.. code-block:: sh

    doit develop_install -c pyviz/label/dev -c conda-forge -c bokeh -o build -o tests -o recommended

The above command installs Panel's dependencies using conda, then
performs a pip editable install of Panel. If it fails, ``nodejs>=14.0.0`` may be missing
from your environment, fix it with ``conda install -c conda-forge nodejs`` then rerun above command.

Developing custom models
------------------------

Panel ships with a number of custom Bokeh models, which have both
Python and Javascript components. When developing Panel these custom
models have to be compiled. This happens automatically with ``pip
install -e .`` or ``python setup.py develop``, however when runnning
actively developing you can rebuild the extension with ``panel build
panel``. The ``build`` command is just an alias for ``bokeh build``; see
the `Bokeh developer guide`_ for more information about developing
bokeh models.

Just like any other Javascript (or Typescript) library Panel defines a
``package.json`` and ``package-lock.json`` files. When adding, updating or
removing a dependency in the package.json file ensure you commit the
changes to the ``package-lock.json`` after running ``npm install``.

Bundling resources
------------------

Panel bundles external resources required for custom models and templates
into the ``panel/dist`` directory. The bundled resources have to be collected
whenever they change, so rerun ``pip
install -e .`` or ``python setup.py develop`` whenever you change one of the following:

* A new model is added with a ``__javascript_raw__`` declaration or an existing model is updated
* A new template with a ``_resources`` declaration is added or an existing template is updated
* A CSS file in one of template directories (``panel/template/*/``) is added or modified

Next Steps
----------

You will likely want to check out the :ref:`devguide_testing` guide. Meanwhile,
if you have any problems with the steps here, please `contact the developers`_.

Useful Links
------------

-  `Dev version of Panel Site`_
    -  Use this to explore new, not yet released features and docs
-  `Panel master branch on Binder`_
    -  Use this to quickly explore and manually test the newest panel features in a fresh environment with all requirements installed.
    -  Replace ``master`` with ``name-of-other-branch`` for other branches.


.. _Anaconda: https://anaconda.com/downloads
.. _contact the developers: https://gitter.im/pyviz/pyviz
.. _conda package manager: https://conda.io/docs/intro.html
.. _conda documentation: https://conda.io/docs/index.html
.. _Download conda: https://docs.conda.io/projects/conda/en/latest/user-guide/install/download.html
.. _Git: https://git-scm.com
.. _GitHub: https://github.com
.. _Installing Git: https://git-scm.com/book/en/v2/Getting-Started-Installing-Git
.. _Pro Git Book: https://git-scm.com/book/en/v2
.. _Bokeh developer guide: https://docs.bokeh.org/en/latest/docs/dev_guide/setup.html
.. _Dev version of Panel Site: https://pyviz-dev.github.io/panel
.. _Panel master branch on Binder: https://mybinder.org/v2/gh/holoviz/panel/master?urlpath=lab/tree/examples


.. toctree::
    :titlesonly:
    :hidden:
    :maxdepth: 2

    Getting Set up <self>
    Testing <testing>
