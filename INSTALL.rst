======================
Daisy Producer INSTALL
======================

Requirements
============

Basic requirements
------------------

- Python_ (tested with Python 2.5)
- Django_ 
- lxml_
- `Daisy Pipeline`_ (use the Pipeline Core Packages)
- XeTex_ (to generate Large Print from LaTeX)
- texlive_ (should contain the extbook and titlesec class which is
  needed for Large Print) 
- `Tiresias LP font`_ (a font designed for Large Print publications)
- `Java Runtime`_ (`Daisy Pipeline`_ needs at least Java 5)
- liblouis_
- A database for example PostgreSQL_ or MySQL_.
- A Python database interface like psycopg2_ or mysqldb_ for example.
- docutils_ to render the help page (which is written in
  reStructuredText Markup).

.. _Python: http://www.python.org
.. _Django: http://www.djangoproject.com
.. _lxml: http://codespeak.net/lxml/index.html
.. _Daisy Pipeline: http://www.daisy.org/projects/pipeline/
.. _XeTex: http://www.tug.org/xetex/
.. _texlive: http://www.tug.org/texlive/
.. _`Tiresias LP font`: http://www.tiresias.org/fonts/lpfont/about_lp.htm
.. _`Java Runtime`: http://www.java.com/en/download/manual.jsp
.. _liblouis: http://code.google.com/p/liblouis/
.. _PostgreSQL: http://www.postgresql.org/
.. _MySQL: http://www.mysql.com/
.. _psycopg2: http://www.initd.org/
.. _mysqldb: http://sourceforge.net/projects/mysql-python
.. _docutils: http://docutils.sourceforge.net

Required packages
~~~~~~~~~~~~~~~~~

In terms of (Debian/Ubuntu) packages this translates to

- python
- python-django
- python-lxml
- python-docutils
- sun-java6-jre
- texlive-xetex
- texlive-latex-extra
- texlive-lang-german
- ttf-tiresias
- xsltproc

For PostgreSQL

- postgresql
- postgresql-client
- python-psycopg2

For MySQL

- mysql-server
- mysql-client
- python-mysqldb

There is a `Debian package for liblouis`_ which could probably be
used. The Daisy Pipeline has not been packaged so far and will have to
be installed somewhere.

.. _Debian package for liblouis: http://packages.debian.org/search?keywords=liblouis&searchon=names&suite=all&section=all


Deployment requirements
-----------------------
- Apache_ (apache2)
- `Python WSGI adapter module for Apache`_ (libapache2-mod-wsgi)

.. _Apache: http://www.apache.org
.. _Python WSGI adapter module for Apache: http://code.google.com/p/modwsgi/

Optional requirements
---------------------
- autodoc_ (package postgresql-autodoc) if you want to generate the ER
  diagrams.

.. _autodoc: http://www.rbt.ca/autodoc/

Installation
============

The following settings have to be adapted for your site:

- DATABASE_ENGINE
- DATABASE_NAME
- DATABASE_USER
- DATABASE_PASSWORD
- DAISY_DEFAULT_PUBLISHER
- DAISY_PIPELINE_PATH
- SECRET_KEY
- TIME_ZONE

When installing under Apache you need to set up URLconf to server the
media files for the admin site. Add the following to urls.py

  (r'^media/(?P<path>.*)$', 'django.views.static.serve',
    {'document_root': os.path.join(PROJECT_DIR, 'public', 'media')}),

and add a link under the public directory

  $ cd $DAISYPRODUCER_HOME/public
  $ ln -s /usr/share/python-support/python-django/django/contrib/admin/media media

Adapt the settings file to your environment.

  $ cd $DAISYPRODUCER_HOME
  $ mv settings_test.py settings.py

For the archive create a directory named archive under the
daisyproducer directory and give www-data write access to it:

  $ cd $DAISYPRODUCER_HOME
  $ mkdir archive
  $ chown www-data archive

Configuration
=============
Once the application is installed you will need to configure the
workflow, the users and the groups. I suggest you start with the
workflow. Daisy Producer comes with a default workflow but you are
free to define your own. In the `admin interface`_ you will be able to
define states and transitions between them.

After you've defined the states and the transitions you will have to
create groups and define which group is responsible for which state.
Only members of a group that is responsible for a state will see
pending jobs in that particular state.

Lastly you will have to assign your users to particular groups to make
sure they see the pending jobs that they are responsible for.

 .. _admin interface: http://127.0.0.1:8000/admin/