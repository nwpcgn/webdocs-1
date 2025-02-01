--- 
title: State Machine 2
sidebar_label: State Machine 2
sidebar_position: 732
---





**JS**


```javascript
import {
    createMachine
} from "xstate";
export const machine = createMachine({
    context: {
        level: 1,
        lives: 3,
        score: 0,
        mapsize: 11,
    },
    id: "game",
    initial: "Initializing",
    states: {
        Initializing: {
            description: "game init",
            meta: {
                status: "init",
            },
            tags: "init",
            on: {
                start: [{
                    target: "Playing",
                    actions: [],
                    meta: {},
                }, ],
            },
        },
        Playing: {
            description: "The game is currently being played. Player input is accepted and the game state updates accordingly.",
            entry: [{
                    type: "initScore",
                },
                {
                    type: "initLives",
                },
                {
                    type: "initLevel",
                },
                {
                    type: "generateMap",
                },
            ],
            on: {
                pause: [{
                    target: "Paused",
                    actions: [],
                }, ],
                end: [{
                    target: "Game Over",
                    actions: [],
                }, ],
            },
            type: "parallel",
        },
        Paused: {
            description: "The game is paused. No player input is accepted and the game state is static.",
            on: {
                resume: [{
                    target: "Playing",
                    actions: [],
                    meta: {},
                }, ],
                end: [{
                    target: "Game Over",
                    actions: [],
                    meta: {},
                }, ],
            },
        },
        "Game Over": {
            description: "The game has ended. This could be due to losing all lives, completing all levels, or a player choice to end the game.",
            entry: {
                type: "storeResult",
            },
            on: {
                restart: [{
                    target: "Initializing",
                    actions: [],
                }, ],
            },
        },
    },
}, {
    actions: {
        initLevel: ({
            context,
            event
        }) => {},
        initLives: ({
            context,
            event
        }) => {},
        initScore: ({
            context,
            event
        }) => {},
        generateMap: ({
            context,
            event
        }) => {},
        storeResult: ({
            context,
            event
        }) => {},
    },
    actors: {},
    guards: {},
    delays: {},
}, );
```


```json
{ "context": { "level": 1, "lives": 3, "score": 0, "mapsize": 11 }, "id": "game", "initial": "Initializing", "states": { "Initializing": { "description": "game init", "meta": { "status": "init" }, "tags": "init", "on": { "start": [ { "target": "Playing", "actions": [], "meta": {} } ] } }, "Playing": { "description": "The game is currently being played. Player input is accepted and the game state updates accordingly.", "entry": [ { "type": "initScore" }, { "type": "initLives" }, { "type": "initLevel" }, { "type": "generateMap" } ], "on": { "pause": [ { "target": "Paused", "actions": [] } ], "end": [ { "target": "Game Over", "actions": [] } ] }, "type": "parallel" }, "Paused": { "description": "The game is paused. No player input is accepted and the game state is static.", "on": { "resume": [ { "target": "Playing", "actions": [], "meta": {} } ], "end": [ { "target": "Game Over", "actions": [], "meta": {} } ] } }, "Game Over": { "description": "The game has ended. This could be due to losing all lives, completing all levels, or a player choice to end the game.", "entry": { "type": "storeResult" }, "on": { "restart": [ { "target": "Initializing", "actions": [] } ] } } } }
```
