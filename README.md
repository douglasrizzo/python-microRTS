# python-microRTS

[![PyPI version](https://badge.fury.io/py/python-microRTS.svg)](https://badge.fury.io/py/python-microRTS)

A python client library for [MicroRTS][microrts].

## Installation

```shell
pip install python-microRTS
```

## Overview

The python client library makes use of the `SocketAI` interface in the MicroRTS library.

By default the socketAI library will connect to `localhost:9898`. The python client is hard-coded to use these values also.

## Usage

### Create a controller

Create a class that inherits from the `pyrts.server.Server` class, in `server.py` and implement the `get_action()` method. It is also a good idea to instantiate your controller and call the `start()` method in the main function of your module, so that you can run this controller as a standalone script later.

```python
from pyrts.server import Server

class AI(server):

    def get_action(state):
        # In here implement the algorithm that takes the state and creates the action
        return [ ..  list of actions]

if __name__ == '__main__':
    ai = AI()
    ai.start()

```

The method must return a list of actions. Each action is a dict composed of a unit ID and the action it should execute. If the action has parameters, they should be passed too. Here is an example of a list of actions:

```python
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
```

For every game tick, the `get_action()` method is called. A list of actions is available in `pyrts.Action`. A list of directions (the only argument taken by some of the actions) is available in `pyrts.Direction`.

If there are no actions to send at a given time step, then an empty list `[]` should be sent. If actions are sent to a subset of the units, the other units must receive `Action.NONE`. Units that are still performing actions from previous turns must also receive `Action.NONE` (a list of these units is 
returned by the `get_busy_units()` method inherited by your controller).

#### Examples

Example controllers can be found in the `examples` directory. Currently the following algorithms are implemented:

* `random_actions.py`: perform random actions for all units
* `random_bot_movement.py`: a random walk for the single worker that starts

### Run your controller

Start your AI class in Python. You should see some logging that looks like this:
```
DEBUG:RTSServer:Socket created
DEBUG:RTSServer:Socket bind complete
DEBUG:RTSServer:Socket now listening
```

Once you see this logging, you can start a [MicroRTS environment][microrts] and it should connect to the control server.

### Start MicroRTS in Client Mode

Since your controller is going to have the role of a server, MicroRTS must be started in Client Mode. In order to do that, change the contents of [config.properties](https://github.com/santiontanon/microrts/blob/master/resources/config.properties) so that `launch_mode=CLIENT`. Make sure all other network parameters are kept as default:
```
server_address=127.0.0.1
server_port=9898
serialization_type=2
```

Start MicroRTS. By now, you should see your Python agent interact with the game.

## Useful functions in the Server class

##### get_busy_units(state)

Get a list of the units that are currently busy performing an action

##### get_available_actions_by_type(unit_type_table, type)

Given the unit type and the unit type table, return a complete list of the possible actions that unit can perform

##### get_unit_type_table()

This function returns the unit type table, which describes the environment.

for example, the environment `basesWorkers16x16.xml` will return:

```json
{
 "moveConflictResolutionStrategy": 1,
 "unitTypes": [
  {
   "maxDamage": 1,
   "producedBy": [],
   "name": "Resource",
   "produces": [],
   "canAttack": false,
   "hp": 1,
   "moveTime": 10,
   "canMove": false,
   "isResource": true,
   "sightRadius": 0,
   "attackRange": 1,
   "harvestAmount": 1,
   "cost": 1,
   "produceTime": 10,
   "attackTime": 10,
   "minDamage": 1,
   "canHarvest": false,
   "isStockpile": false,
   "harvestTime": 10,
   "ID": 0,
   "returnTime": 10
  },
  {
   "maxDamage": 1,
   "producedBy": [
    "Worker"
   ],
   "name": "Base",
   "produces": [
    "Worker"
   ],
   "canAttack": false,
   "hp": 10,
   "moveTime": 10,
   "canMove": false,
   "isResource": false,
   "sightRadius": 5,
   "attackRange": 1,
   "harvestAmount": 1,
   "cost": 10,
   "produceTime": 250,
   "attackTime": 10,
   "minDamage": 1,
   "canHarvest": false,
   "isStockpile": true,
   "harvestTime": 10,
   "ID": 1,
   "returnTime": 10
  },
  {
   "maxDamage": 1,
   "producedBy": [
    "Worker"
   ],
   "name": "Barracks",
   "produces": [
    "Light",
    "Heavy",
    "Ranged"
   ],
   "canAttack": false,
   "hp": 4,
   "moveTime": 10,
   "canMove": false,
   "isResource": false,
   "sightRadius": 3,
   "attackRange": 1,
   "harvestAmount": 1,
   "cost": 5,
   "produceTime": 200,
   "attackTime": 10,
   "minDamage": 1,
   "canHarvest": false,
   "isStockpile": false,
   "harvestTime": 10,
   "ID": 2,
   "returnTime": 10
  },
  {
   "maxDamage": 1,
   "producedBy": [
    "Base"
   ],
   "name": "Worker",
   "produces": [
    "Base",
    "Barracks"
   ],
   "canAttack": true,
   "hp": 1,
   "moveTime": 10,
   "canMove": true,
   "isResource": false,
   "sightRadius": 3,
   "attackRange": 1,
   "harvestAmount": 1,
   "cost": 1,
   "produceTime": 50,
   "attackTime": 5,
   "minDamage": 1,
   "canHarvest": true,
   "isStockpile": false,
   "harvestTime": 20,
   "ID": 3,
   "returnTime": 10
  },
  {
   "maxDamage": 2,
   "producedBy": [
    "Barracks"
   ],
   "name": "Light",
   "produces": [],
   "canAttack": true,
   "hp": 4,
   "moveTime": 8,
   "canMove": true,
   "isResource": false,
   "sightRadius": 2,
   "attackRange": 1,
   "harvestAmount": 1,
   "cost": 2,
   "produceTime": 80,
   "attackTime": 5,
   "minDamage": 2,
   "canHarvest": false,
   "isStockpile": false,
   "harvestTime": 10,
   "ID": 4,
   "returnTime": 10
  },
  {
   "maxDamage": 4,
   "producedBy": [
    "Barracks"
   ],
   "name": "Heavy",
   "produces": [],
   "canAttack": true,
   "hp": 4,
   "moveTime": 12,
   "canMove": true,
   "isResource": false,
   "sightRadius": 2,
   "attackRange": 1,
   "harvestAmount": 1,
   "cost": 2,
   "produceTime": 120,
   "attackTime": 5,
   "minDamage": 4,
   "canHarvest": false,
   "isStockpile": false,
   "harvestTime": 10,
   "ID": 5,
   "returnTime": 10
  },
  {
   "maxDamage": 1,
   "producedBy": [
    "Barracks"
   ],
   "name": "Ranged",
   "produces": [],
   "canAttack": true,
   "hp": 1,
   "moveTime": 10,
   "canMove": true,
   "isResource": false,
   "sightRadius": 3,
   "attackRange": 3,
   "harvestAmount": 1,
   "cost": 2,
   "produceTime": 100,
   "attackTime": 5,
   "minDamage": 1,
   "canHarvest": false,
   "isStockpile": false,
   "harvestTime": 10,
   "ID": 6,
   "returnTime": 10
  }
 ]
}
```

##### get_valid_action_positions_for_state(state):

Returns a tuple containing the following:

* *invalid_move_positions*: a set of all the positions that cannot be moved into
* *valid_harvest_positions*:  a set of all the resource locations
* *valid_base_positions*: a set of the positions of bases on the current players team
* *valid_attack_positions*: a set of the positions that can be attacked

These positions can be cross-referenced with possible actions that units can perform, to make sure no invalid
actions are sent to the environment

##### get_valid_actions_for_unit(unit, available_actions, valid_positions)

Get the actions that are valid for a unit to perform.
An action is INVALID if the action cannot be performed in the environment.
For example, if the action is MOVE(left) but the position to the left of the unit is blocked

##### get_available_actions_by_type_name(unit_type_table, type_name)

Gets a list of the available actions that can be performed by a particlar unit

##### get_resources_for_player(state, player)

Get the number of resources the player currently has available

##### get_resource_usage_from_state(state)

How many resources are currently being used to build units

##### get_resource_usage_from_actions(actions)

From a list of actions, sum the cost of the actions

## Cite

If you want to cite this library, please use the following DOI:

[![DOI](https://zenodo.org/badge/149242629.svg)](https://zenodo.org/badge/latestdoi/149242629)

[microrts]: https://github.com/santiontanon/microrts