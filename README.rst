==========
python.sg
==========

This project is a community effort of the local Python community in Singapore.

Contributing
=============

If you want to contribute to this project, you need to follow a few simple 
steps:

- Fork this repository
- Create a feature branch
- Implement your changes
- Send us a pull request
  
Installation
=============

After forking this project, the following commands should get you a working
local development environment quickly (assuming you use virtualenvwrapper)::

    mkvirtualenv -p python2.7 pythonsg
    workon pythonsg
    cd into the root directory of your pythonsg clone
    pip install --upgrade -r requirements.txt
    cd proj
    cp local_settings.py.sample local_settings.py
    ./manage.py collectstatic
    cd ../proj_public/ 
    mkdir media
    cd media
    ln -s /path/to/your/venv/lib/python2.7/site-packages/cms/media/cms
    ln -s /path/to/your/venv/lib/python2.7/site-packages/filer/media/filer
    cd ../../proj/

If you want to do some serious development, you should also consider to 
install additional packages. These will install PIL, psycopg2, ipdb,
ipython and more::

    pip install --upgrade -r dev_requirements.txt

Database Set-up
=====================

This project will use postgresql9.0 onwards on our production server.  

Do make sure that you have that installed on your local machine.  Things will
still work in general if you do not install postgresql and use sqlite3 but
that's not encouraged because we do not intend to support questions /
clarifications about databases which are not used on our production server.

On your linux ubuntu, you should be able to install postgresql by running::
   
    sudo aptitude install postgresql-9.0 postgresql-server-dev-9.0

The above does not work for Ubuntu 11.04. We have yet to find a way to install 
it properly.
 
If you are using a Mac, and you are using MacPorts::

    sudo port install postgresql90 postgresql90-server

Once you have your postgresql database installed locally, create your local db
as follows.

1. local_settings.py
-------------------------------

In your local_settings.py file, make sure you specify your postgresql database
in the format -

::

    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.postgresql_psycopg2',
            'NAME': 'whateveryourlocalpostgresqldbis',
            'USER': 'whateveryourlocalpostgresqluseris',
            'PASSWORD': 'whateveryourpasswordis',
            'HOST': '',
        }
    }


2. Create the corresponding postgresql database name, user and password
---------------------------------------------------------------------------

::

    createuser -U postgres whateveryourlocalpostgresqluseris -P  # No to superuser, Yes to create new database and No to create more new roles
    createdb -U whateveryourlocalpostgresqluseris -E utf8 -O whateveryourlocalpostgresqluseris whateveryourlocalpostgresqldbis -T template0

3. Initialize your database
----------------------------

::

    ./manage.py syncdb --migrate


Finally
=====================

::

./manage.py runserver

Loading initial data
=====================

If you want to load your fresh database with some initial testdata, you can use
our fixtures. In this case you don't need to create a superuser. It is included
in the fixture (admin, test123)::

  ./manage.py reset contenttypes
  ./manage.py loaddata fixtures/bootstrap.json

The bootstrap fixtures have been created with the following command::

  ./manage.py dumpdata --natural contenttypes auth cms text cmsplugin_blog cmsplugin_pygments > fixtures/bootstrap.json


Our remote postgresql database will be made available for access only for core
developers involved in this project.
