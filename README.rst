python-microRTS
===============

|PyPI version|

A python client library for
`microRTS <https://github.com/santiontanon/microrts>`__.

This project is a fork of the
`original python-microRTS <https://github.com/Bam4d/python-microRTS>`__, by
Chris Bramford. As per the Apache License 2.0, a copy of the license is kept in
the project and changes in the code (which are still minor) are documented in
the `commit history <https://github.com/douglasrizzo/python-microRTS>`__ of the
project.

If you're on GitHub and want to see
**usage examples, API documentation and message descriptions**, access the
project documentation `here <https://douglasrizzo.com.br/python-microRTS>`__.

Installation
------------

This version of python-microRTS is not available on PyPI. To install it
directly from GitHub:

.. code:: shell

    pip install git+https://github.com/douglasrizzo/python-microRTS.git

Overview
--------

By running microRTS in client mode, it will allow an external process to
send actions to agents via a socket interface, as well as update the
external process with the current state of the game. python-microRTS makes
use of this feature, more specifically the ``SocketAI`` interface in the
microRTS library.

By default the SocketAI library will connect to ``localhost:9898``. The
python client is hard-coded to use these values also.

States are received as JSON strings, which are converted to Python ``dict``
objects. An action is created as a Python ``list`` of ``dict`` s, which is
converted to a JSON string for socket communication.

Cite
----

If you want to cite this library, please use the following DOI:

|DOI|

.. |PyPI version| image:: https://badge.fury.io/py/python-microRTS.svg
   :target: https://badge.fury.io/py/python-microRTS
.. |DOI| image:: https://zenodo.org/badge/149242629.svg
   :target: https://zenodo.org/badge/latestdoi/149242629
