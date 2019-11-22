python-microRTS
===============

|PyPI version|

A python client library for
`microRTS <https://github.com/santiontanon/microrts>`__.

This project is a fork of the `original python-microRTS <https://github.com/Bam4d/python-microRTS>`__, by Chris Bramford. As per the Apache License 2.0, a copy of the license is kept in the project and changes in the code (which are still minor) are documented in the `commit history <https://github.com/douglasrizzo/python-microRTS>`__ of the project.

If you're on GitHub, you can view the project documentation `here <https://douglasrizzo.com.br/python-microRTS>`__.

Installation
------------

.. code:: shell

   pip install python-microRTS

Overview
--------

By running microRTS in client mode, it will allow an external process to
send actions to agents via a socket interface, as well as update the
external process with the current state of the game. python-microRTS makes
use of this feature, more specifically the ``SocketAI`` interface in the
microRTS library.

By default the SocketAI library will connect to ``localhost:9898``. The
python client is hard-coded to use these values also.

:ref:`States <State>` are received as JSON strings, which are converted
to Python ``dict`` objects. An :ref:`action <Action>` is created as a
Python ``list`` of ``dict`` s, which is converted to a JSON string for
socket communication.

Usage
-----

Create a controller
~~~~~~~~~~~~~~~~~~~

Create a class that inherits from the :class:`pyrts.server.Server` class,
in, pass the ID of the player (0 or 1) and implement the
:py:func:`get_action() <pyrts.server.Server.get_action()>` method. It is also
a good idea to instantiate your controller and call the
:py:func:`start() <pyrts.server.Server.start()>` method in the main function of
your module, so that you can run this controller as a standalone script later.

.. code:: python

   from pyrts.server import Server

   class AI(server):

        def __init__(self, player_id):
            super(AI, self).__init__(player_id)

        def get_action(state):
            # In here implement the algorithm that takes the state and creates the action
            return [ ..  list of actions]

    if __name__ == '__main__':
        ai = AI(0)
        ai.start()

The method must return a ``list`` of actions, one action for each unit in the
game. The list is a standard Python list and each action is a Python ``dict``
objects containing the unit ID and another ``dict`` containing the action and
its parameters. Action names and codes are listed in
:class:`pyrts.server.Action`. Action parameters, which are mostly directions to
apply the actions, are listed in :class:`pyrts.server.Direction`.

.. code:: python

   [
       {
           "unitID": 0 # the unit that you wish to control
           "unitAction": {
               'type': 1 # the type of action (this is defined in the unit type table)
               'parameters': 0 # the parameters for the action (this is defined in the unit type table)
           }
       },
       {
           # more actions for different unit IDs
       }
   ]

For every game tick, the ``get_action()`` method is called. A list of
actions is available in ``pyrts.Action``. A list of directions (the only
argument taken by some of the actions) is available in
``pyrts.Direction``.

If there are no actions to send at a given time step, then an empty list
``[]`` should be sent. If actions are sent to a subset of the units, the
other units must receive ``Action.NONE``. Units that are still
performing actions from previous turns must also receive ``Action.NONE``
(a list of these units is returned by the ``get_busy_units()`` method
inherited by your controller).

Examples
^^^^^^^^

Example controllers can be found in the ``examples`` directory.
Currently the following algorithms are implemented:

-  ``random_actions.py``: perform random actions for all units
-  ``random_bot_movement.py``: a random walk for the single worker that
   starts

Run your controller
~~~~~~~~~~~~~~~~~~~

Start your AI class in Python. You should see some logging that looks
like this:

::

   DEBUG:RTSServer:Socket created
   DEBUG:RTSServer:Socket bind complete
   DEBUG:RTSServer:Socket now listening

Once you see this logging, you can start a `microRTS
environment <https://github.com/santiontanon/microrts>`__ and it should
connect to the control server.

Start microRTS in Client Mode
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Since your controller is going to have the role of a server, microRTS
must be started in Client Mode. In order to do that, change the contents
of
`config.properties <https://github.com/santiontanon/microrts/blob/master/resources/config.properties>`__
so that ``launch_mode=CLIENT``. Make sure all other network parameters
are kept as default:

::

   server_address=127.0.0.1
   server_port=9898
   serialization_type=2

Start microRTS. By now, you should see your Python agent interact with
the game.

Cite
----

If you want to cite this library, please use the following DOI:

|DOI|

.. |PyPI version| image:: https://badge.fury.io/py/python-microRTS.svg
   :target: https://badge.fury.io/py/python-microRTS
.. |DOI| image:: https://zenodo.org/badge/149242629.svg
   :target: https://zenodo.org/badge/latestdoi/149242629
