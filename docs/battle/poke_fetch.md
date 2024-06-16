---
title: Pokemon API Fetch
sidebar_position: 5
sidebar_label: Pkmn API
---



## Pokemons Fetch

data[]

*keys* 

`name, sprites, types`

### Sprites

`data.sprites{}  key: 'https:// ...'`

*keys* 
`back_default, back_shiny, front_default, front_shiny`


### Types

data.types[] `{ slot, type = {name, url} }`

*keys* 

`slot, name = type.name`

## Generator Funktion

```js title="generator.js"
const outputSet = new Set()
let outputObj = {}

async function doFetchGen() {
    let {
        name,
        sprites,
        types
    } = await fetchData(randNum(1, 400))
    let output = Object.assign({}, {
        name,
        sprites,
        types
    })
    delete output.sprites.other
    delete output.sprites.versions
    let newSpriteObj = {}
    for (const key in output.sprites) {
        if (Object.hasOwnProperty.call(output.sprites, key)) {
            const url = output.sprites[key]
            if (url && !key.includes('female')) {
                newSpriteObj[key] = url
            }
        }
    }
    output.sprites = newSpriteObj
    outputObj = output
    outputSet.add(output)
    return output
}
```


*helper*

```js title="helper.js"
const fetchData = async (n = 1) => {
    try {
        const res = await fetch(`https://pokeapi.co/api/v2/pokemon/${n}`)
        const data = await res.json()
        fetchedNumber = number
        return data
    } catch (error) {
        throw new Error(error)
    }
}

const randNum = (min, max) => {
    min = Math.ceil(min)
    max = Math.floor(max)
    return Math.floor(Math.random() * (max - min)) + min
}
```

## Example

```json title="~pokemon.json"
{
    "name": "machoke",
    "sprites": {
        "back_default": "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/back/67.png",
        "back_shiny": "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/back/shiny/67.png",
        "front_default": "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/67.png",
        "front_shiny": "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/shiny/67.png"
    },
    "types": [
        {
            "slot": 1,
            "type": {
                "name": "fighting",
                "url": "https://pokeapi.co/api/v2/type/2/"
            }
        }
    ]
}
```

## API Data

```json title="pokemon.min.json"
[{"name":"phanpy","sprites":{"back_default":"https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/back/231.png","back_shiny":"https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/back/shiny/231.png","front_default":"https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/231.png","front_shiny":"https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/shiny/231.png"},"types":[{"slot":1,"type":{"name":"ground","url":"https://pokeapi.co/api/v2/type/5/"}}]},{"name":"ursaring","sprites":{"back_default":"https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/back/217.png","back_shiny":"https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/back/shiny/217.png","front_default":"https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/217.png","front_shiny":"https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/shiny/217.png"},"types":[{"slot":1,"type":{"name":"normal","url":"https://pokeapi.co/api/v2/type/1/"}}]},{"name":"sudowoodo","sprites":{"back_default":"https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/back/185.png","back_shiny":"https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/back/shiny/185.png","front_default":"https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/185.png","front_shiny":"https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/shiny/185.png"},"types":[{"slot":1,"type":{"name":"rock","url":"https://pokeapi.co/api/v2/type/6/"}}]},{"name":"jolteon","sprites":{"back_default":"https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/back/135.png","back_shiny":"https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/back/shiny/135.png","front_default":"https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/135.png","front_shiny":"https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/shiny/135.png"},"types":[{"slot":1,"type":{"name":"electric","url":"https://pokeapi.co/api/v2/type/13/"}}]},{"name":"hoothoot","sprites":{"back_default":"https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/back/163.png","back_shiny":"https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/back/shiny/163.png","front_default":"https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/163.png","front_shiny":"https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/shiny/163.png"},"types":[{"slot":1,"type":{"name":"normal","url":"https://pokeapi.co/api/v2/type/1/"}},{"slot":2,"type":{"name":"flying","url":"https://pokeapi.co/api/v2/type/3/"}}]},{"name":"slugma","sprites":{"back_default":"https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/back/218.png","back_shiny":"https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/back/shiny/218.png","front_default":"https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/218.png","front_shiny":"https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/shiny/218.png"},"types":[{"slot":1,"type":{"name":"fire","url":"https://pokeapi.co/api/v2/type/10/"}}]},{"name":"paras","sprites":{"back_default":"https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/back/46.png","back_shiny":"ht
```