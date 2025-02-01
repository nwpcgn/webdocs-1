--- 
title: Origin DB
tags: 
    - Rpg-Game
sidebar_label: Origin
sidebar_position: 10
---

# IncreMental Rpg


## Daten


```json title="rpg-data.json"
{
  "titel": "RPG",
  "resourcen": [
    {
      "slug": "wood",
      "name": "Wood",
      "description": "Brown stuff that grows on trees.",
      "action": "Chop",
      "total": 0,
      "clickIncrement": 1,
      "autoIncrement": 0,
      "max": 100,
      "storage": {
        "total": 1,
        "max": 100,
        "cost": {"wood": 50, "stone": 50, "iron": 0, "food": 0},
        "costIncrease": 1.1
      }
    },
    {
      "slug": "stone",
      "name": "Stone",
      "description": "Hard stuff.",
      "action": "Mine",
      "total": 0,
      "clickIncrement": 1,
      "autoIncrement": 0,
      "max": 100,
      "storage": {
        "total": 1,
        "max": 100,
        "cost": {"wood": 50, "stone": 50},
        "costIncrease": 1.1
      }
    },
    {
      "slug": "iron",
      "name": "Iron",
      "description": "Even harder stuff. Bit rusty.",
      "action": "Mine",
      "total": 0,
      "autoIncrement": 0,
      "max": 100,
      "storage": {
        "total": 1,
        "max": 100,
        "cost": {"wood": 100, "stone": 100},
        "costIncrease": 1.1
      },
      "chanceIncrement": 1
    },
    {
      "slug": "food",
      "name": "Food",
      "description": "Can be cooked to provide nutrition.",
      "action": "Hunt",
      "total": 0,
      "clickIncrement": 1,
      "autoIncrement": 0,
      "max": 100,
      "storage": {
        "total": 1,
        "max": 100,
        "cost": {"wood": 50, "stone": 50},
        "costIncrease": 1.1
      },
      "chanceIncrement": 1
    }
  ],
  "buildings": [
    {
      "slug": "tent",
      "name": "Tent",
      "description": "Just like a house but way smaller and made out of fabric.",
      "total": 0,
      "residents": 1,
      "cost": {"wood": 30, "stone": 0, "iron": 0},
      "costIncrease": 1.1
    },
    {
      "slug": "house",
      "name": "House",
      "description": "Just like a tent but way bigger and not made out of fabric.",
      "total": 0,
      "residents": 4,
      "cost": {"wood": 75, "stone": 50, "iron": 10, "food": 0},
      "costIncrease": 1.1
    },
    {
      "slug": "hostel",
      "name": "Hostel",
      "description": "A bit like a house but not as nice and not as private.",
      "total": 0,
      "residents": 10,
      "cost": {"wood": 200, "stone": 30, "iron": 30},
      "costIncrease": 1.1
    }
  ],
  "worker": [
    {
      "slug": "lumberjack",
      "name": "Lumberjack",
      "description": "A person who likes to chop wood.",
      "resourcen": "wood",
      "total": 0,
      "autoIncrement": 1,
      "cost": {"food": 10},
      "costIncrease": 1.1
    },
    {
      "slug": "miner",
      "name": "Miner",
      "description": "Not a young person.",
      "resourcen": "stone",
      "total": 0,
      "autoIncrement": 1,
      "cost": {"wood": 0, "stone": 0, "iron": 0, "food": 10},
      "costIncrease": 1.1
    },
    {
      "slug": "scraper",
      "name": "Scraper",
      "description": "Isn't it obvious?",
      "resourcen": "iron",
      "total": 0,
      "autoIncrement": 1,
      "cost": {"stone": 1, "food": 10},
      "costIncrease": 1.2
    },
    {
      "slug": "hunter",
      "name": "Hunter",
      "description": "The opposite to a gatherer.",
      "resourcen": "food",
      "total": 0,
      "autoIncrement": 1,
      "cost": {"food": 10},
      "costIncrease": 1.1
    }
  ],
  "meta": {
    "city": "Gotham City",
    "name": "Bruce Wayne",
    "population": 0,
    "maxPopulation": 0,
    "tick": 1000,
    "saveGameInterval": 30000
  }
}

```

## Schema



```json title="rpg-schema.json"
{
	"type": "object",
	"$schema": "http://json-schema.org/draft-07/schema#",
	"description": "JSON schema generated with JSONBuddy https://www.json-buddy.com",
	"properties": {
		"titel": {
			"type": "string"
		},
		"resourcen": {
			"type": "array",
			"items": {
				"$ref": "#/definitions/Resourcen"
			}
		},
		"buildings": {
			"type": "array",
			"items": {
				"$ref": "#/definitions/Building"
			}
		},
		"worker": {
			"type": "array",
			"items": {
				"$ref": "#/definitions/Worker"
			}
		},
		"meta": {
			"$ref": "#/definitions/Meta"
		}
	},
	"definitions": {
		"Building": {
			"type": "object",
			"additionalProperties": false,
			"properties": {
				"slug": {
					"type": "string"
				},
				"name": {
					"type": "string"
				},
				"description": {
					"type": "string"
				},
				"total": {
					"type": "integer"
				},
				"residents": {
					"type": "integer"
				},
				"cost": {
					"$ref": "#/definitions/Cost"
				},
				"costIncrease": {
					"type": "number"
				}
			},
			"required": [
				"cost",
				"costIncrease",
				"description",
				"name",
				"residents",
				"slug",
				"total"
			],
			"title": "Building"
		},
		"Cost": {
			"type": "object",
			"additionalProperties": false,
			"properties": {
				"wood": {
					"type": "integer"
				},
				"stone": {
					"type": "integer"
				},
				"iron": {
					"type": "integer"
				},
				"food": {
					"type": "integer"
				}
			},
			"required": [  ],
			"title": "Cost"
		},
		"Meta": {
			"type": "object",
			"additionalProperties": false,
			"properties": {
				"city": {
					"type": "string"
				},
				"name": {
					"type": "string"
				},
				"population": {
					"type": "integer"
				},
				"maxPopulation": {
					"type": "integer"
				},
				"tick": {
					"type": "integer"
				},
				"saveGameInterval": {
					"type": "integer"
				}
			},
			"required": [
				"city",
				"maxPopulation",
				"name",
				"population",
				"saveGameInterval",
				"tick"
			],
			"title": "Meta"
		},
		"Resourcen": {
			"type": "object",
			"additionalProperties": false,
			"properties": {
				"slug": {
					"type": "string"
				},
				"name": {
					"type": "string"
				},
				"description": {
					"type": "string"
				},
				"action": {
					"type": "string"
				},
				"total": {
					"type": "integer"
				},
				"clickIncrement": {
					"type": "integer"
				},
				"autoIncrement": {
					"type": "integer"
				},
				"max": {
					"type": "integer"
				},
				"storage": {
					"$ref": "#/definitions/Storage"
				},
				"chanceIncrement": {
					"type": "integer"
				}
			},
			"required": [
				"action",
				"autoIncrement",
				"description",
				"max",
				"name",
				"slug",
				"storage",
				"total"
			],
			"title": "Resourcen"
		},
		"Storage": {
			"type": "object",
			"additionalProperties": false,
			"properties": {
				"total": {
					"type": "integer"
				},
				"max": {
					"type": "integer"
				},
				"cost": {
					"$ref": "#/definitions/Cost"
				},
				"costIncrease": {
					"type": "number"
				}
			},
			"required": [ "cost", "costIncrease", "max", "total" ],
			"title": "Storage"
		},
		"Worker": {
			"type": "object",
			"additionalProperties": false,
			"properties": {
				"slug": {
					"type": "string"
				},
				"name": {
					"type": "string"
				},
				"description": {
					"type": "string"
				},
				"resourcen": {
					"type": "string"
				},
				"total": {
					"type": "integer"
				},
				"autoIncrement": {
					"type": "integer"
				},
				"cost": {
					"$ref": "#/definitions/Cost"
				},
				"costIncrease": {
					"type": "number"
				}
			},
			"required": [
				"autoIncrement",
				"cost",
				"costIncrease",
				"description",
				"name",
				"resourcen",
				"slug",
				"total"
			],
			"title": "Worker"
		}
	}
}
```


## Typescript


```javascript
export interface Welcome1 {
    titel:     string;
    resourcen:  Resourcen[];
    buildings: Building[];
    worker:    Worker[];
    meta:      Meta;
}

export interface Building {
    slug:         string;
    name:         string;
    description:  string;
    total:        number;
    residents:    number;
    cost:         Cost;
    costIncrease: number;
}

export interface Cost {
    wood?:  number;
    stone?: number;
    iron?:  number;
    food?:  number;
}

export interface Meta {
    city:             string;
    name:             string;
    population:       number;
    maxPopulation:    number;
    tick:             number;
    saveGameInterval: number;
}

export interface Resourcen {
    slug:             string;
    name:             string;
    description:      string;
    action:           string;
    total:            number;
    clickIncrement?:  number;
    autoIncrement:    number;
    max:              number;
    storage:          Storage;
    chanceIncrement?: number;
}

export interface Storage {
    total:        number;
    max:          number;
    cost:         Cost;
    costIncrease: number;
}

export interface Worker {
    slug:          string;
    name:          string;
    description:   string;
    resourcen:      string;
    total:         number;
    autoIncrement: number;
    cost:          Cost;
    costIncrease:  number;
}
```


