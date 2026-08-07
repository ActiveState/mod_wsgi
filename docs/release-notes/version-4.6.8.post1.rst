===================
Version 4.6.8.post1
===================

Version 4.6.8.post1 is an ActiveState security release of mod_wsgi 4.6.8. It
contains no functional changes other than the security fix described below.

Note that the ``mod_wsgi.version`` tuple exposed to WSGI applications is
deliberately left as ``(4, 6, 8)`` so that existing version comparisons keep
working. Only the version string, as reported in the ``Server`` header and
used for packaging, becomes ``4.6.8.post1``.

Security Fixes
--------------

* **CVE-2022-2255** (GHSA-7527-8855-9cf8)

  When using ``WSGITrustedProxies`` and ``WSGITrustedProxyHeaders`` in the
  Apache configuration, or the ``--trust-proxy`` and ``--trust-proxy-header``
  options with ``mod_wsgi-express``, if you trusted the ``X-Client-IP``
  header and a request was received from an untrusted client, the header was
  not being correctly removed from the set of headers passed through to the
  WSGI application.

  This only occurred with the ``X-Client-IP`` header; the same problem was
  not present when trusting the ``X-Real-IP`` or ``X-Forwarded-For`` headers.

  ``REMOTE_ADDR`` was correctly left untouched for untrusted clients, so a
  WSGI application which follows best practice and reads only ``REMOTE_ADDR``
  was not affected. An application which additionally enabled WSGI or web
  framework middleware that re-processes proxy headers could however be
  induced to trust a client-supplied address.

  This backports the upstream fix released in mod_wsgi 4.9.3.
