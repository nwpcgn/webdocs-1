---
title: Svelte Note I
sidebar_position: 28
sidebar_label: Svelte Note I
---


## Notifications



### notification.svelte


```html title="notification.svelte"
<script lang="ts">
	import { fade, slide } from 'svelte/transition'
	import { getNote, log } from './log.svelte.ts'
	function typewriter(node, { speed = 4 }) {
		const valid =
			node.childNodes.length === 1 &&
			node.childNodes[0].nodeType === Node.TEXT_NODE

		if (!valid) {
			throw new Error(
				`This transition only works on elements with a single text node child`
			)
		}

		const text = node.textContent
		const duration = text.length / (speed * 0.01)

		return {
			duration,
			tick: (t) => {
				const i = ~~(text.length * t)
				node.textContent = text.slice(0, i)
			}
		}
	}
	let frame: HTMLDivElement | null = $state(null)
	let fh = $state(0)
	let fw = $state(0)
	let fch: number = $state(0)
	let { label, colorClass = 'bg-base-100 text-base-content' } = $props()
	const addNote = async () => {
		const obj = getNote()
		log.addMessage(obj)
	}
	const clearList = () => {
		log.empty()
	}
	$effect(async () => {
		if (frame) {
			log.init(frame)
		}

		if (fch) {
			frame.scrollTo({ top: fch, behavior: 'smooth' })
		}
	})
	const headerClass = `${colorClass}`
	const boxClass = `overflow-x-hidden overflow-y-auto size-[360px]`
</script>

{#snippet msgLine({ id, type, message, dismissible })}
	<div
		transition:slide|global
		role="alert"
		class="alert"
		class:alert-warning={type === 'warning'}
		class:alert-info={type === 'info'}
		class:alert-error={type === 'error'}
		class:alert-success={type === 'success'}>
		{#if dismissible}
			<button
				style="--fs: 16px;"
				aria-label="close"
				onclick={() => {
					log.remove(id)
				}}
				class="size-4 cursor-pointer rounded-box text-sm">
				&#10006;
			</button>
		{/if}
		<span out:fade={{ duration: 200 }} in:typewriter>{message}</span>
	</div>
{/snippet}

{#if label}
	<header class={headerClass}>
		<div class="padded-small flex items-center gap-2">
			<div class="h4">
				<span
					class="transition-opacity duration-300 ease-in"
					class:opacity-40={log.busy}>{label}</span>
				{#if log.messages.length}
					<span class="text-lg text-accent" in:fade
						>+{log.messages.length}</span>
				{:else if log.list.length}
					<span class="text-lg" in:fade>{log.list.length}</span>
				{/if}
			</div>

			<span class="flex-1"></span>
			<nav class="grid grid-cols-2 gap-2">
				<button onclick={addNote} aria-label="addNote" class="cursor-pointer">
					&#10010;
				</button>
				<button
					disabled={log.busy}
					onclick={clearList}
					aria-label="clearNotes"
					class="cursor-pointer">
					&#10006;
				</button>
			</nav>
		</div>
	</header>
{/if}

<div
	class={boxClass}
	bind:this={frame}
	bind:clientHeight={fh}
	bind:clientWidth={fw}>
	<div class="grid gap-2" bind:clientHeight={fch}>
		{#each log.list as note (note.id)}
			{@render msgLine(note)}
		{:else}
			<div class="padded-small grid place-content-center">
				<span>no Messages</span>
			</div>
		{/each}
	</div>
</div>
```



### log.svelte.ts


```typescript title="log.svelte.ts"
import alerts from './alerts.json' // [{"order":"1","type":"info","message":"Item erscheint plötzlich in deiner Tasche.","dismissible":true,"timeout":5000}]
const uuidv4 = function (length = 6) {
	return Math.random()
		.toString(36)
		.substring(2, length + 2)
}
interface Note {
	id: string
	type: string
	message: string
	dismissible: boolean
	timeout: number
}
export class MsgLogger {
	messages: Note[] = $state([])
	list: Note[] = $state([])
	currentIndex: number = $state(0)
	timeId: number = $state(0)
	notificationWindow: HTMLElement | null = $state(null)
	#busy: boolean = $state(false)

	get busy() {
		return this.#busy
	}

	set busy(value) {
		this.#busy = value
	}

	init(notificationWindow: HTMLElement) {
		this.notificationWindow = notificationWindow
	}

	addMessage(message: Note) {
		const id = uuidv4()
		const defaults = {
			id,
			type: 'info',
			dismissible: true,
			timeout: 10000
		}
		const mObj = { ...defaults, ...message }
		this.messages.push(mObj)

		if (this.messages.length === 1) {
			this.showNextMessage()
		}
	}
	dismissMessage(id: string) {
		this.list = this.list.filter((t) => t.id !== id)
	}

	async showNextMessage() {
		if (this.#busy) return
		if (!this.messages.length) {
			clearTimeout(this.timeId)
			this.#busy = false

			return
		}

		this.#busy = true
		const speed = 4
		const msg = this.messages.shift()
		const text = msg?.message
		const duration = text.length / (speed * 0.01)
		this.list.push(msg)
		this.timeId = setTimeout(() => {
			if (msg.timeout)
				setTimeout(() => this.dismissMessage(msg.id), msg.timeout)
			this.#busy = false
			this.showNextMessage()
		}, duration)
	}

	remove(id: string) {
		this.list = this.list.filter((t) => t.id !== id)
	}

	empty() {
		if (this.#busy) return
		// this.scrollDown()
		if (!this.list.length) {
			clearTimeout(this.timeId)
			this.#busy = false

			return
		}

		this.#busy = true
		this.list.shift()
		this.timeId = setTimeout(() => {
			this.#busy = false
			this.empty()
		}, 400)
	}

	clear() {
		clearTimeout(this.timeId)
		this.messages = []
		this.list = []
		this.#busy = false
	}
}

export const log = new MsgLogger()

export function getNote(): Note {
	const i = Math.floor(Math.random() * alerts.length)
	const obj = alerts[i]
	const { type, message, dismissible, timeout } = obj
	return { type, message, dismissible, timeout }
}

export default log
```