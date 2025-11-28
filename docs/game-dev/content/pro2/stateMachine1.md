--- 
title: State Machine 1
sidebar_label: State Machine 1
sidebar_position: 731
---




-   [Get started](https://stately.ai/docs/category/get-started)
-   Cheatsheet

Version: XState v5

# Cheatsheet

Use this cheatsheet to quickly look up the syntax for XState v5.
stateMachine1
## Installing XState[​](https://stately.ai/docs/cheatsheet#installing-xstate "Direct link to heading")

-   npm
-   pnpm
-   yarn

```
npm install xstate
```

[Read more on installing XState](https://stately.ai/docs/installation).

## Creating a state machine[​](https://stately.ai/docs/cheatsheet#creating-a-state-machine "Direct link to heading")

```javascript
import { setup, createActor, assign } from 'xstate';

const machine = setup({/* ... */})
  .createMachine({
    id: 'toggle',
    initial: 'active',
    context: { count: 0 },
    states: {
      active: {
        entry: assign({
          count: ({ context }) => context.count + 1,
        }),
        on: {
          toggle: { target: 'inactive' },
        },
      },
      inactive: {
        on: {
          toggle: { target: 'active' },
        },
      },
    },
  });

const actor = createActor(machine);
actor.subscribe((snapshot) => {
  console.log(snapshot.value);
});

actor.start();
// logs 'active' with context { count: 1 }

actor.send({ type: 'toggle' });
// logs 'inactive' with context { count: 1 }
actor.send({ type: 'toggle' });
// logs 'active' with context { count: 2 }
actor.send({ type: 'toggle' });
// logs 'inactive' with context { count: 2 }
```

[Read more about the actor model](https://stately.ai/docs/actor-model).

## Creating promise logic[​](https://stately.ai/docs/cheatsheet#creating-promise-logic "Direct link to heading")

```
import { fromPromise, createActor } from 'xstate';

const promiseLogic = fromPromise(async () => {
  const response = await fetch('https://dog.ceo/api/breeds/image/random');
  const dog = await response.json();
  return dog;
});

const actor = createActor(promiseLogic);

actor.subscribe((snapshot) => {
  console.log(snapshot);
});

actor.start();
// logs: {
//   message: "https://images.dog.ceo/breeds/kuvasz/n02104029_110.jpg",
//   status: "success"
// }
```

[Read more about promise actor logic](https://stately.ai/docs/actors#actors-as-promises).

## Creating transition logic[​](https://stately.ai/docs/cheatsheet#creating-transition-logic "Direct link to heading")

A transition function is just like a reducer.

```
import { fromTransition, createActor } from 'xstate';

const transitionLogic = fromTransition(
  (state, event) => {
    switch (event.type) {
      case 'inc':
        return {
          ...state,
          count: state.count + 1,
        };
      default:
        return state;
    }
  },
  { count: 0 }, // initial state
);

const actor = createActor(transitionLogic);

actor.subscribe((snapshot) => {
  console.log(snapshot);
});

actor.start();
// logs { count: 0 }

actor.send({ type: 'inc' });
// logs { count: 1 }
actor.send({ type: 'inc' });
// logs { count: 2 }
```

[Read more about transition actors](https://stately.ai/docs/actors#fromtransition).

## Creating observable logic[​](https://stately.ai/docs/cheatsheet#creating-observable-logic "Direct link to heading")

```
import { fromTransition, createActor } from 'xstate';

const transitionLogic = fromTransition(
  (state, event) => {
    switch (event.type) {
      case 'inc':
        return {
          ...state,
          count: state.count + 1,
        };
      default:
        return state;
    }
  },
  { count: 0 }, // initial state
);

const actor = createActor(transitionLogic);

actor.subscribe((snapshot) => {
  console.log(snapshot);
});

actor.start();
// logs { count: 0 }

actor.send({ type: 'inc' });
// logs { count: 1 }
actor.send({ type: 'inc' });
// logs { count: 2 }
```

[Read more about observable actors](https://stately.ai/docs/actors#fromobservable).

## Creating callback logic[​](https://stately.ai/docs/cheatsheet#creating-callback-logic "Direct link to heading")

```
import { setup, createActor } from 'xstate';

const machine = setup({/* ... */})
  .createMachine({
    id: 'parent',
    initial: 'active',
    states: {
      active: {
        initial: 'one',
        states: {
          one: {
            on: {
              NEXT: { target: 'two' }
            }
          },
          two: {},
        },
        on: {
          NEXT: { target: 'inactive' }
        }
      },
      inactive: {},
    },
  });

const actor = createActor(machine);

actor.subscribe((snapshot) => {
  console.log(snapshot.value);
});

actor.start();
// logs { active: 'one' }

actor.send({ type: 'NEXT' });
// logs { active: 'two' }

actor.send({ type: 'NEXT' });
// logs 'inactive'
```

[Read more about callback actors](https://stately.ai/docs/actors#fromcallback).

## Parent states[​](https://stately.ai/docs/cheatsheet#parent-states "Direct link to heading")

```
import { setup, createActor } from 'xstate';

const machine = setup({
  actions: {
    activate: () => {/* ... */},
    deactivate: () => {/* ... */},
    notify: (_, params: { message: string }) => {/* ... */},
  }
}).createMachine({
  id: 'toggle',
  initial: 'active',
  states: {
    active: {
      entry: { type: 'activate' },
      exit: { type: 'deactivate' },
      on: {
        toggle: {
          target: 'inactive',
          actions: [{ type: 'notify' }],
        },
      },
    },
    inactive: {
      on: {
        toggle: {
          target: 'active',
          actions: [
            // action with params
            {
              type: 'notify',
              params: {
                message: 'Some notification',
              },
            },
          ],
        },
      },
    },
  },
});

const actor = createActor(
  machine.provide({
    actions: {
      notify: (_, params) => {
        console.log(params.message ?? 'Default message');
      },
      activate: () => {
        console.log('Activating');
      },
      deactivate: () => {
        console.log('Deactivating');
      },
    },
  }),
);

actor.start();
// logs 'Activating'

actor.send({ type: 'toggle' });
// logs 'Deactivating'
// logs 'Default message'

actor.send({ type: 'toggle' });
// logs 'Some notification'
// logs 'Activating'
```

[Read more about parent states](https://stately.ai/docs/parent-states).

## Actions[​](https://stately.ai/docs/cheatsheet#actions "Direct link to heading")

```
import { setup, createActor } from 'xstate';const machine = setup({  actions: {    activate: () => {/* ... */},    deactivate: () => {/* ... */},    notify: (_, params: { message: string }) => {/* ... */},  }}).createMachine({  id: 'toggle',  initial: 'active',  states: {    active: {      entry: { type: 'activate' },      exit: { type: 'deactivate' },      on: {        toggle: {          target: 'inactive',          actions: [{ type: 'notify' }],        },      },    },    inactive: {      on: {        toggle: {          target: 'active',          actions: [            // action with params            {              type: 'notify',              params: {                message: 'Some notification',              },            },          ],        },      },    },  },});const actor = createActor(  machine.provide({    actions: {      notify: (_, params) => {        console.log(params.message ?? 'Default message');      },      activate: () => {        console.log('Activating');      },      deactivate: () => {        console.log('Deactivating');      },    },  }),);actor.start();// logs 'Activating'actor.send({ type: 'toggle' });// logs 'Deactivating'// logs 'Default message'actor.send({ type: 'toggle' });// logs 'Some notification'// logs 'Activating'
```

[Read more about actions](https://stately.ai/docs/actions).

## Guards[​](https://stately.ai/docs/cheatsheet#guards "Direct link to heading")

```
import { setup, createActor } from 'xstate';const machine = setup({  guards: {    canBeToggled: ({ context }) => context.canActivate,    isAfterTime: (_, params) => {      const { time } = params;      const [hour, minute] = time.split(':');      const now = new Date();      return now.getHours() > hour && now.getMinutes() > minute;    },  },  actions: {    notifyNotAllowed: () => {      console.log('Cannot be toggled');    },  },}).createMachine({  id: 'toggle',  initial: 'active',  context: {    canActivate: false,  },  states: {    inactive: {      on: {        toggle: [          {            target: 'active',            guard: 'canBeToggled',          },          {            actions: 'notifyNotAllowed',          },        ],      },    },    active: {      on: {        toggle: {          // Guard with params          guard: { type: 'isAfterTime', params: { time: '16:00' } },          target: 'inactive',        },      },      // ...    },  },});const actor = createActor(machine);actor.start();// logs 'Cannot be toggled'
```

[Read more about guards](https://stately.ai/docs/guards).

## Invoking actors[​](https://stately.ai/docs/cheatsheet#invoking-actors "Direct link to heading")

```
import { setup, fromPromise, createActor, assign } from 'xstate';const loadUserLogic = fromPromise(async () => {  const response = await fetch('https://jsonplaceholder.typicode.com/users/1');  const user = await response.json();  return user;});const machine = setup({  actors: { loadUserLogic }}).createMachine({  id: 'toggle',  initial: 'loading',  context: {    user: undefined,  },  states: {    loading: {      invoke: {        id: 'loadUser',        src: 'loadUserLogic',        onDone: {          target: 'doSomethingWithUser',          actions: assign({            user: ({ event }) => event.output,          }),        },        onError: {          target: 'failure',          actions: ({ event }) => {            console.log(event.error)          }        }      },    },    doSomethingWithUser: {      // ...    },    failure: {      // ...    },  },});const actor = createActor(machine);actor.subscribe((snapshot) => {  console.log(snapshot.context.user);});actor.start();// eventually logs:// { id: 1, name: 'Leanne Graham', ... }
```

[Read more about invoking actors](https://stately.ai/docs/invoke).

## Spawning actors[​](https://stately.ai/docs/cheatsheet#spawning-actors "Direct link to heading")

```
import { setup, fromPromise, createActor, assign } from 'xstate';const loadUserLogic = fromPromise(async () => {  const response = await fetch('https://jsonplaceholder.typicode.com/users/1');  const user = await response.json();  return user;});const machine = setup({  actors: {    loadUserLogic  }}).createMachine({  context: {    userRef: undefined,  },  on: {    loadUser: {      actions: assign({        userRef: ({ spawn }) => spawn('loadUserLogic'),      }),    },  },});const actor = createActor(machine);actor.subscribe((snapshot) => {  const { userRef } = snapshot.context;  console.log(userRef?.getSnapshot());});actor.start();actor.send({ type: 'loadUser' });// eventually logs:// { id: 1, name: 'Leanne Graham', ... }
```

[Read more about spawning actors](https://stately.ai/docs/spawn).

## Input and output[​](https://stately.ai/docs/cheatsheet#input-and-output "Direct link to heading")

```
import { setup, createActor } from 'xstate';const greetMachine = setup({  types: {    context: {} as { message: string },    input: {} as { name: string },  }}).createMachine({  context: ({ input }) => ({    message: `Hello, ${input.name}`,  }),  entry: ({ context }) => {    console.log(context.message);  },});const actor = createActor(greetMachine, {  input: {    name: 'David',  },});actor.start();// logs 'Hello, David'
```

[Read more about input](https://stately.ai/docs/input).

## Invoking actors with input[​](https://stately.ai/docs/cheatsheet#invoking-actors-with-input "Direct link to heading")

```
import { setup, createActor, fromPromise } from 'xstate';const loadUserLogic = fromPromise(async ({ input }) => {  const response = await fetch(    `https://jsonplaceholder.typicode.com/users/${input.id}`,  );  const user = await response.json();  return user;});const machine = setup({  actors: {    loadUserLogic  }}).createMachine({  initial: 'loading user',  states: {    'loading user': {      invoke: {        id: 'loadUser',        src: 'loadUserLogic',        input: {          id: 3,        },        onDone: {          actions: ({ event }) => {            console.log(event.output);          },        },      },    },  },});const actor = createActor(machine);actor.start();// eventually logs:// { id: 3, name: 'Clementine Bauch', ... }
```

[Read more about invoking actors with input](https://stately.ai/docs/input#invoking-actors-with-input).

## Types[​](https://stately.ai/docs/cheatsheet#types "Direct link to heading")

```
import { setup, fromPromise } from 'xstate';const promiseLogic = fromPromise(async () => {  /* ... */});const machine = setup({  types: {    context: {} as {      count: number;    };    events: {} as       | { type: 'inc'; }      | { type: 'dec' }      | { type: 'incBy'; amount: number };    actions: {} as       | { type: 'notify'; params: { message: string } }      | { type: 'handleChange' };    guards: {} as       | { type: 'canBeToggled' }      | { type: 'isAfterTime'; params: { time: string } };    children: {} as {      promise1: 'someSrc';      promise2: 'someSrc';    };    delays: 'shortTimeout' | 'longTimeout';    tags: 'tag1' | 'tag2';    input: number;    output: string;  },  actors: {    promiseLogic  }}).createMachine({  // ...});
```

[Edit this page on GitHub](https://github.com/statelyai/docs/tree/main/docs/cheatsheet.mdx)

[](https://stately.ai/docs/templates)

Previous

Templates

[  
](https://stately.ai/docs/typescript)
