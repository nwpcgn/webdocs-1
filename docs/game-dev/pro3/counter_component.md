---
sidebar_label: Counter
title: Counter Component
sidebar_position: 201
tags: [Svelte, Js]
---


# Counter Component


---


![previeww](https://i.imgur.com/wPB9qjU.png)

---

## JS

```javascript title="stepper.js"
import { writable, get } from 'svelte/store'

const clamp = (min, max, v) => {
    if (v <= min) {
        return min
    }
    if (v >= max) {
        return max
    }

    return v
}
export const minCount = writable(0)
export const maxCount = writable(100)
export const clickCount = writable(1)

function createCountStore() {
    const { subscribe, set, update } = writable(0)
    const min = get(minCount)
    const max = get(maxCount)
    return {
        subscribe,
        inc: () =>
            update((n) => {
                const value = n + get(clickCount)
                return clamp(get(minCount), get(maxCount), value)
            }),
        dec: (val = 1) =>
            update((n) => {
                const min = get(minCount)
                const sub = n - val
                if (sub >= get(minCount)) return sub

                return n
            }),
        reset: () => set(min)
    }
}

export const count = createCountStore()


```


## Svelte


```html title="Stepper.svelte"
<script>
    import { minCount, maxCount, clickCount, count } from './stores/stepper'
    let step = $clickCount
    let min = $minCount
    let max = $maxCount
    let cost = 1

    function upgradeInc(e) {
        clickCount.set(step)
    }
</script>

<section class="flex flex-col items-center gap-4 p-8 shadow-lg rounded-box">
    <header>
        <span class="text-6xl font-extrabold">{$count}</span>
    </header>
    <div class="flex gap-2 justify-center">
        <button class="btn btn-circle" on:click={() => count.dec(cost)}>-</button>
        <button class="btn btn-circle" on:click={count.inc}>+</button>
    </div>

    <details class="collapse bg-base-200">
        <summary class="collapse-title font-medium text-left">Options</summary>
        <div class="collapse-content">
            <section class="py-4">
                <label class="label">
                    <span class="label-text">Min</span>
                    <input
                        class="input input-bordered input-sm"
                        type="number"
                        on:change={(e) => {
                            const v = parseInt(e.currentTarget.value)
                            minCount.set(v)
                        }}
                        value={min}
                        {max} />
                </label>

                <label class="label">
                    <span class="label-text">Max</span>
                    <input
                        class="input input-bordered input-sm"
                        type="number"
                        on:change={(e) => {
                            const v = parseInt(e.currentTarget.value)
                            maxCount.set(v)
                        }}
                        value={max}
                        {min} />
                </label>

                <label class="label">
                    <span class="label-text">Step</span>
                    <input
                        class="input input-bordered input-sm"
                        type="number"
                        on:change={(e) => {
                            const v = parseInt(e.currentTarget.value)
                            clickCount.set(v)
                        }}
                        value={step}
                        min={0} />
                </label>
            </section>
        </div>
    </details>
</section>

```