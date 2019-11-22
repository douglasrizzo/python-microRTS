Message examples
================

Unit type table
---------------

According to the microRTS documentation:

    The unit type table stores the unit types the game can have. It also determines the attributes of each unit type. The unit type table determines the balance of the game.

Below is an example of a unit type table message in JSON format, which is sent to the Python controller at the beginning of a game. This message starts with the ``utt`` identifier.

.. code-block:: json

    {
      "moveConflictResolutionStrategy": 1,
      "unitTypes": [
        {
          "ID": 0,
          "name": "Resource",
          "cost": 1,
          "hp": 1,
          "minDamage": 1,
          "maxDamage": 1,
          "attackRange": 1,
          "produceTime": 10,
          "moveTime": 10,
          "attackTime": 10,
          "harvestTime": 10,
          "returnTime": 10,
          "harvestAmount": 1,
          "sightRadius": 0,
          "isResource": true,
          "isStockpile": false,
          "canHarvest": false,
          "canMove": false,
          "canAttack": false,
          "produces": [],
          "producedBy": []
        },
        {
          "ID": 1,
          "name": "Base",
          "cost": 10,
          "hp": 10,
          "minDamage": 1,
          "maxDamage": 1,
          "attackRange": 1,
          "produceTime": 200,
          "moveTime": 10,
          "attackTime": 10,
          "harvestTime": 10,
          "returnTime": 10,
          "harvestAmount": 1,
          "sightRadius": 5,
          "isResource": false,
          "isStockpile": true,
          "canHarvest": false,
          "canMove": false,
          "canAttack": false,
          "produces": [
            "Worker"
          ],
          "producedBy": [
            "Worker"
          ]
        },
        {
          "ID": 2,
          "name": "Barracks",
          "cost": 5,
          "hp": 4,
          "minDamage": 1,
          "maxDamage": 1,
          "attackRange": 1,
          "produceTime": 100,
          "moveTime": 10,
          "attackTime": 10,
          "harvestTime": 10,
          "returnTime": 10,
          "harvestAmount": 1,
          "sightRadius": 3,
          "isResource": false,
          "isStockpile": false,
          "canHarvest": false,
          "canMove": false,
          "canAttack": false,
          "produces": [
            "Light",
            "Heavy",
            "Ranged"
          ],
          "producedBy": [
            "Worker"
          ]
        },
        {
          "ID": 3,
          "name": "Worker",
          "cost": 1,
          "hp": 1,
          "minDamage": 1,
          "maxDamage": 1,
          "attackRange": 1,
          "produceTime": 50,
          "moveTime": 10,
          "attackTime": 5,
          "harvestTime": 20,
          "returnTime": 10,
          "harvestAmount": 1,
          "sightRadius": 3,
          "isResource": false,
          "isStockpile": false,
          "canHarvest": true,
          "canMove": true,
          "canAttack": true,
          "produces": [
            "Base",
            "Barracks"
          ],
          "producedBy": [
            "Base"
          ]
        },
        {
          "ID": 4,
          "name": "Light",
          "cost": 2,
          "hp": 4,
          "minDamage": 2,
          "maxDamage": 2,
          "attackRange": 1,
          "produceTime": 80,
          "moveTime": 8,
          "attackTime": 5,
          "harvestTime": 10,
          "returnTime": 10,
          "harvestAmount": 1,
          "sightRadius": 2,
          "isResource": false,
          "isStockpile": false,
          "canHarvest": false,
          "canMove": true,
          "canAttack": true,
          "produces": [],
          "producedBy": [
            "Barracks"
          ]
        },
        {
          "ID": 5,
          "name": "Heavy",
          "cost": 3,
          "hp": 8,
          "minDamage": 4,
          "maxDamage": 4,
          "attackRange": 1,
          "produceTime": 120,
          "moveTime": 10,
          "attackTime": 5,
          "harvestTime": 10,
          "returnTime": 10,
          "harvestAmount": 1,
          "sightRadius": 2,
          "isResource": false,
          "isStockpile": false,
          "canHarvest": false,
          "canMove": true,
          "canAttack": true,
          "produces": [],
          "producedBy": [
            "Barracks"
          ]
        },
        {
          "ID": 6,
          "name": "Ranged",
          "cost": 2,
          "hp": 1,
          "minDamage": 1,
          "maxDamage": 1,
          "attackRange": 3,
          "produceTime": 100,
          "moveTime": 10,
          "attackTime": 5,
          "harvestTime": 10,
          "returnTime": 10,
          "harvestAmount": 1,
          "sightRadius": 3,
          "isResource": false,
          "isStockpile": false,
          "canHarvest": false,
          "canMove": true,
          "canAttack": true,
          "produces": [],
          "producedBy": [
            "Barracks"
          ]
        }
      ]
    }

State
-----

A state represents the current state of the game. This message starts with the ``getAction`` identifier. After receiving a state, the controller may send an action back to microRTS.

.. code-block:: json

    {
      "time": 0,
      "pgs": {
        "width": 16,
        "height": 16,
        "terrain": "0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
        "players": [
          {
            "ID": 0,
            "resources": 5
          },
          {
            "ID": 1,
            "resources": 5
          }
        ],
        "units": [
          {
            "type": "Resource",
            "ID": 16,
            "player": -1,
            "x": 0,
            "y": 3,
            "resources": 25,
            "hitpoints": 1
          },
          {
            "type": "Resource",
            "ID": 17,
            "player": -1,
            "x": 0,
            "y": 4,
            "resources": 25,
            "hitpoints": 1
          },
          {
            "type": "Resource",
            "ID": 18,
            "player": -1,
            "x": 15,
            "y": 11,
            "resources": 25,
            "hitpoints": 1
          },
          {
            "type": "Resource",
            "ID": 19,
            "player": -1,
            "x": 15,
            "y": 12,
            "resources": 25,
            "hitpoints": 1
          },
          {
            "type": "Base",
            "ID": 20,
            "player": 0,
            "x": 2,
            "y": 5,
            "resources": 0,
            "hitpoints": 10
          },
          {
            "type": "Base",
            "ID": 21,
            "player": 1,
            "x": 13,
            "y": 10,
            "resources": 0,
            "hitpoints": 10
          },
          {
            "type": "Worker",
            "ID": 22,
            "player": 0,
            "x": 1,
            "y": 4,
            "resources": 0,
            "hitpoints": 1
          },
          {
            "type": "Worker",
            "ID": 23,
            "player": 1,
            "x": 14,
            "y": 11,
            "resources": 0,
            "hitpoints": 1
          }
        ]
      },
      "actions": []
    }

Action
------

Every time the :py:func:`pyrts.server.Server.get_action()` method of the user-created controller is called, a list of actions must be returned, one action for each unit in the game. The list is a standard Python list and each action is a Python ``dict`` objects containing the unit ID and another ``dict`` containing the action and its parameters. Action names and codes are listed in :class:`pyrts.server.Action`. Action parameters, which are mostly directions to apply the actions, are listed in :class:`pyrts.server.Direction`.

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
