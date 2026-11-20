===================
Version 4.6.8.post2
===================

Version 4.6.8.post2 is an ActiveState packaging release of mod_wsgi 4.6.8. It
carries the CVE-2022-2255 fix from 4.6.8.post1 and changes only how
``setup.py`` and ``mod_wsgi-express`` find Apache. The Apache module itself is
unchanged.

As with 4.6.8.post1, the ``mod_wsgi.version`` tuple stays ``(4, 6, 8)``. Only
the version string, as reported in the ``Server`` header and used for
packaging, becomes ``4.6.8.post2``.

Features Changed
----------------

* ``mod_wsgi-express`` now uses the Apache from the ``mod_wsgi-httpd`` package
  whenever that package is installed. Before, it did so only when mod_wsgi had
  been compiled against ``mod_wsgi-httpd``: a build compiled against another
  Apache ignored the package, and a build compiled against the package failed
  with ``ImportError`` once the package was removed.

  One build of mod_wsgi now serves both uses. Installed on its own, it is an
  Apache module to load into an existing Apache with ``LoadModule``, and
  ``mod_wsgi-express`` falls back to the Apache it was compiled against.
  Installed together with ``mod_wsgi-httpd``, ``mod_wsgi-express`` runs
  that package's Apache.

* ``setup.py`` can now compile against ``mod_wsgi-httpd`` when the package is
  importable but its ``mod_wsgi-apxs`` script is not installed, for example
  when the package is on ``PYTHONPATH`` in a build sandbox. It reads the Apache
  configuration from the package in-process, getting the same answers the
  ``mod_wsgi-apxs`` script would give. Setting ``APXS``, or having
  ``mod_wsgi-apxs`` next to the Python interpreter, still takes precedence.
