---
title: Svelte Wheel I
sidebar_position: 29
sidebar_label: Svelte Wheel I
---


## Wheel of Fortune




```html title"wheel.svelte"
<script lang="ts">
	import { onMount } from 'svelte'
	let wheel = $state()
	let spin = $state()
	let ctx = $state()
	let isAccelerating = $state(false)
	let animFrame = null
	let {
		sectors,
		isSpinning = $bindable(false),
		onTurnStart,
		onTurnEnd
	} = $props()
	let result = $state({ color: '#f82', label: 'Stack' })
	const tot = sectors.length
	const dia = 350
	const rad = dia / 2
	const PI = Math.PI
	const TAU = 2 * PI
	const arc = TAU / tot
	const friction = 0.991
	const angVelMin = 0.002
	let angVelMax = $state(0)
	let angVel = $state(0)
	let ang = $state(0)
	const rand = (m, M) => Math.random() * (M - m) + m
	const getIndex = () => Math.floor(tot - (ang / TAU) * tot) % tot

	function drawSector(sector, i) {
		const ang = arc * i
		ctx.save()
		// COLOR
		ctx.beginPath()
		ctx.fillStyle = sector.color
		ctx.moveTo(rad, rad)
		ctx.arc(rad, rad, rad, ang, ang + arc)
		ctx.lineTo(rad, rad)
		ctx.fill()
		// TEXT
		ctx.translate(rad, rad)
		ctx.rotate(ang + arc / 2)
		ctx.textAlign = 'right'
		ctx.fillStyle = '#fff'
		ctx.font = 'bold 22px sans-serif'
		ctx.fillText(sector.name, rad - 10, 10)
		ctx.restore()
	}

	function rotate() {
		const sector = sectors[getIndex()]
		wheel.style.transform = `rotate(${ang - PI / 2}rad)`
		spin.textContent = !angVel ? 'SPIN' : sector.name
		spin.style.background = sector.color
		result = { ...sector }
	}

	function frame() {
		if (!isSpinning) return

		if (angVel >= angVelMax) isAccelerating = false

		if (isAccelerating) {
			angVel ||= angVelMin
			angVel *= 1.06
		} else {
			isAccelerating = false
			angVel *= friction

			if (angVel < angVelMin) {
				isSpinning = false
				angVel = 0
				cancelAnimationFrame(animFrame)
				onTurnEnd(result)
			}
		}

		ang += angVel
		ang %= TAU
		rotate()
	}

	function engine() {
		frame()
		animFrame = requestAnimationFrame(engine)
	}

	function handleSpin() {
		if (isSpinning) return
		isSpinning = true
		isAccelerating = true
		angVelMax = rand(0.5, 0.75)
		onTurnStart()
		engine()
	}

	onMount(() => {
		ctx = wheel.getContext('2d')
		sectors.forEach(drawSector)
		rotate()

		return () => {
			isSpinning = false
			angVel = 0
			cancelAnimationFrame(animFrame)
		}
	})
</script>

<div id="wheelOfFortune">
	<canvas bind:this={wheel} id="wheel" width={dia} height={dia}></canvas>
	<button disabled={isSpinning} bind:this={spin} id="spin" onclick={handleSpin}
		>SPIN</button>
</div>

<style>
	#wheelOfFortune {
		position: relative;
		overflow: hidden;
	}

	#wheel {
		display: block;
	}

	#spin {
		font: 1.5rem/0 sans-serif;
		user-select: none;
		cursor: pointer;
		display: flex;
		justify-content: center;
		align-items: center;
		position: absolute;
		top: 50%;
		left: 50%;
		width: 30%;
		height: 30%;
		margin: -15%;
		background: #fff;
		color: #fff;
		box-shadow:
			0 0 0 8px currentColor,
			0 0px 15px 5px rgba(0, 0, 0, 0.6);
		border-radius: 50%;
		transition: 0.8s;
		opacity: 1;
	}

	#spin::after {
		content: '';
		position: absolute;
		top: -17px;
		border: 10px solid transparent;
		border-bottom-color: currentColor;
		border-top: none;
	}
</style>
```