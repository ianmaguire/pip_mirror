This application implements a PyPI mirror application
according to PEP 381.

Installation
------------

It is possible to run this application directly from the source
distribution. Alternatively, 'python setup.py install' could be used.

The actual mirroring is performed by the pip_mirrorrun script, which
should be invoked through cron like this::

   */15 *  *   *   *     /path/pip_mirror/pip_mirror -q /var/pypi

The command line arguments points to root of the data files that
the mirror creates. An initial run (without the -q option) should
be performed manually. It is possible to interrupt the mirroring;
it will automatically know where to continue when restarted.

In above example, /var/pypi/web must be served through the webserver.
An Apache configuration could read like this::

  <VirtualHost IPADDRESS:80>
    ServerName X.pypi.python.org
    CustomLog /var/log/apache2/pypi.log combined
    DocumentRoot /var/pypi/web
    SetEnv PYPITARGET /var/pypi
    ScriptAlias /sync /path/pip_mirrorsync.cgi
  </VirtualHost>

Notice that supporting the sync URL requires that the web server
user has access to the mirror data, or else that the CGI script
runs as the mirror user.

To propagate the download statistics back to the central server,
processlogs must be run regularly, e.g. through::

   10 7  *   *   *     /path/pip_mirror/processlogs /var/pypi /var/log/apache2/pypi.log{,.1}

Contact
-------

If you have questions or comments, please submit a bug report to
https://github.com/ianmaguire/pip_mirror/issues, or contact me
at mr.scalability@gmail.com

Changes
-------
1.0 (2018-01-25):

- Forked from pep381_client
