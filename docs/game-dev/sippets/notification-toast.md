---
id: Notification 1
title: Notification 1 
sidebar_label: Notes 1
sidebar_position: 1001
---


# Notification Component



## Main Component

```html title='Notes.svelte'
<script>
	import './app.css'
	import Notifications from './notifications.svelte'

	let notificationsComponent = $state()

	const sampleNotifications = [
		{
			id: 1,
			message: 'Welcome! Your account has been successfully created.',
			type: 'success'
		},
		{
			id: 2,
			message: 'New message received from John Doe.',
			type: 'info'
		},
		{
			id: 3,
			message: 'Your subscription will expire in 3 days.',
			type: 'warning'
		},
		{
			id: 4,
			message: 'Failed to save changes. Please try again.',
			type: 'error'
		},
		{
			id: 5,
			message: 'System maintenance scheduled for tonight at 2 AM.',
			type: 'info'
		},
		{
			id: 6,
			message:
				'Are you sure you want to delete this item? This action cannot be undone.',
			type: 'confirm',
			confirmText: 'Delete',
			cancelText: 'Cancel',
			callback: (confirmed) => {
				if (confirmed) {
					notificationsComponent?.addNotification({
						id: Date.now(),
						message: 'Item deleted successfully!',
						type: 'success'
					})
				} else {
					notificationsComponent?.addNotification({
						id: Date.now(),
						message: 'Delete action cancelled.',
						type: 'info'
					})
				}
			}
		},
		{
			id: 7,
			message: 'Do you want to save your changes before leaving?',
			type: 'confirm',
			confirmText: 'Save & Leave',
			cancelText: 'Discard',
			callback: (confirmed) => {
				if (confirmed) {
					notificationsComponent?.addNotification({
						id: Date.now(),
						message: 'Changes saved successfully!',
						type: 'success'
					})
				} else {
					notificationsComponent?.addNotification({
						id: Date.now(),
						message: 'Changes discarded.',
						type: 'warning'
					})
				}
			}
		}
	]

	function addNotification() {
		const randomNotification =
			sampleNotifications[
				Math.floor(Math.random() * sampleNotifications.length)
			]

		const newNotification = {
			...randomNotification,
			id: Date.now() + Math.random()
		}

		notificationsComponent?.addNotification(newNotification)
	}

	function addMultipleNotifications() {
		sampleNotifications.forEach((notification, index) => {
			setTimeout(() => {
				const newNotification = {
					...notification,
					id: Date.now() + Math.random() + index
				}

				notificationsComponent?.addNotification(newNotification)
			}, index * 200)
		})
	}

	function addConfirmDialog() {
		const confirmNotifications = sampleNotifications.filter(
			(n) => n.type === 'confirm'
		)
		const randomConfirm =
			confirmNotifications[
				Math.floor(Math.random() * confirmNotifications.length)
			]

		const newNotification = {
			...randomConfirm,
			id: Date.now() + Math.random()
		}

		notificationsComponent?.addNotification(newNotification)
	}

	function addCustomConfirm() {
		notificationsComponent?.addNotification({
			id: Date.now(),
			message:
				'This is a custom confirm dialog. Do you want to proceed with this action?',
			type: 'confirm',
			confirmText: 'Yes, Proceed',
			cancelText: 'No, Cancel',
			callback: (confirmed) => {
				const message = confirmed
					? 'You confirmed the action!'
					: 'You cancelled the action.'
				const type = confirmed ? 'success' : 'info'

				notificationsComponent?.addNotification({
					id: Date.now() + 1,
					message,
					type
				})
			}
		})
	}
</script>

<div class="min-h-screen bg-gray-50 p-8">
	<div class="mx-auto max-w-4xl">
		<h1 class="mb-8 text-3xl font-bold text-gray-900">Notifications Demo</h1>

		<div class="mb-8 flex flex-wrap space-y-2 space-x-4">
			<button
				onclick={addNotification}
				class="rounded-lg bg-blue-600 px-4 py-2 text-white transition-colors hover:bg-blue-700">
				Add Random Notification
			</button>

			<button
				onclick={addMultipleNotifications}
				class="rounded-lg bg-green-600 px-4 py-2 text-white transition-colors hover:bg-green-700">
				Add Multiple Notifications
			</button>

			<button
				onclick={addConfirmDialog}
				class="rounded-lg bg-purple-600 px-4 py-2 text-white transition-colors hover:bg-purple-700">
				Add Confirm Dialog
			</button>

			<button
				onclick={addCustomConfirm}
				class="rounded-lg bg-orange-600 px-4 py-2 text-white transition-colors hover:bg-orange-700">
				Add Custom Confirm
			</button>
		</div>

		<div class="rounded-lg bg-white p-6 shadow-lg">
			<h2 class="mb-4 text-xl font-semibold">Notification Area</h2>
			<p class="mb-4 text-gray-600">
				Click the buttons above to see notifications appear with typing
				animation.
			</p>

			<!-- Notifications will appear here -->
		</div>
	</div>

	<Notifications bind:this={notificationsComponent} />
</div>

```

## Noteification Child


```html title='notifications.svelte'
<script>
	import { onMount } from 'svelte'

	let notifications = $state([])
	let currentNotification = $state(null)
	let displayedText = $state('')
	let isTyping = $state(false)
	let isVisible = $state(false)

	const TYPING_SPEED = 50 // milliseconds per character
	const DISPLAY_DURATION = 4000 // how long to show the notification after typing (not used for confirm)
	const FADE_DURATION = 300 // fade out duration
	const CONFIRM_TYPING_SPEED = 30 // faster typing for confirm dialogs

	let typingTimeout
	let displayTimeout
	let fadeTimeout

	// Process the notification queue
	const processQueue = () => {
		if (notifications.length > 0 && !currentNotification) {
			currentNotification = notifications[0]
			notifications = notifications.slice(1)
			startTypingAnimation()
		}
	}

	// Start the typing animation
	const startTypingAnimation = () => {
		if (!currentNotification) return

		displayedText = ''
		isTyping = true
		isVisible = true

		const message = currentNotification.message
		const speed =
			currentNotification.type === 'confirm'
				? CONFIRM_TYPING_SPEED
				: TYPING_SPEED
		let charIndex = 0

		const typeNextChar = () => {
			if (charIndex < message.length) {
				displayedText += message[charIndex]
				charIndex++
				typingTimeout = setTimeout(typeNextChar, speed)
			} else {
				isTyping = false
				// Only start auto-dismiss timer for non-confirm notifications
				if (currentNotification.type !== 'confirm') {
					displayTimeout = setTimeout(
						removeCurrentNotification,
						DISPLAY_DURATION
					)
				}
			}
		}

		typeNextChar()
	}

	// Remove the current notification
	const removeCurrentNotification = () => {
		isVisible = false

		fadeTimeout = setTimeout(() => {
			currentNotification = null
			displayedText = ''
			// Process next notification in queue
			processQueue()
		}, FADE_DURATION)
	}

	// Handle confirm dialog response
	const handleConfirmResponse = (confirmed) => {
		if (currentNotification?.callback) {
			currentNotification.callback(confirmed)
		}
		removeCurrentNotification()
	}

	// Manual dismiss function (updated)
	const dismissNotification = () => {
		clearTimeout(typingTimeout)
		clearTimeout(displayTimeout)

		// For confirm dialogs, treat dismiss as cancel
		if (
			currentNotification?.type === 'confirm' &&
			currentNotification?.callback
		) {
			currentNotification.callback(false)
		}

		removeCurrentNotification()
	}

	// Public method to add notifications
	export const addNotification = (notification) => {
		notifications = [...notifications, notification]
		processQueue()
	}

	// Watch for changes in notifications array
	$effect(() => {
		processQueue()
	})

	// Cleanup timeouts on component destroy
	onMount(() => {
		return () => {
			clearTimeout(typingTimeout)
			clearTimeout(displayTimeout)
			clearTimeout(fadeTimeout)
		}
	})

	// Get notification styles based on type
	const getNotificationStyles = (type) => {
		const baseStyles = 'border-l-4 shadow-lg'

		switch (type) {
			case 'success':
				return `${baseStyles} bg-green-50 border-green-400 text-green-800`
			case 'error':
				return `${baseStyles} bg-red-50 border-red-400 text-red-800`
			case 'warning':
				return `${baseStyles} bg-yellow-50 border-yellow-400 text-yellow-800`
			case 'confirm':
				return `${baseStyles} bg-purple-50 border-purple-400 text-purple-800`
			case 'info':
			default:
				return `${baseStyles} bg-blue-50 border-blue-400 text-blue-800`
		}
	}

	// Get icon based on notification type
	const getIcon = (type) => {
		switch (type) {
			case 'success':
				return '✓'
			case 'error':
				return '✕'
			case 'warning':
				return '⚠'
			case 'confirm':
				return '?'
			case 'info':
			default:
				return 'ℹ'
		}
	}
</script>

<!-- Notification Container -->
{#if currentNotification}
	<div
		class="fixed top-4 right-4 z-50 w-full max-w-sm transform transition-all duration-300 ease-in-out"
		class:opacity-100={isVisible}
		class:opacity-0={!isVisible}
		class:translate-y-0={isVisible}
		class:-translate-y-2={!isVisible}
		role={currentNotification.type === 'confirm' ? 'dialog' : 'alert'}
		aria-live="polite"
		aria-atomic="true"
		aria-modal={currentNotification.type === 'confirm' ? 'true' : 'false'}>
		<div
			class={`rounded-lg p-4 ${getNotificationStyles(currentNotification.type)}`}>
			<div class="flex items-start">
				<!-- Icon -->
				<div class="mr-3 flex-shrink-0">
					<span class="text-lg font-bold" aria-hidden="true">
						{getIcon(currentNotification.type)}
					</span>
				</div>

				<!-- Message Content -->
				<div class="min-w-0 flex-1">
					<p class="text-sm leading-5 font-medium">
						{displayedText}
						{#if isTyping}
							<span class="animate-pulse">|</span>
						{/if}
					</p>
				</div>

				<!-- Close Button (only for non-confirm notifications) -->
				{#if currentNotification.type !== 'confirm'}
					<div class="ml-3 flex-shrink-0">
						<button
							onclick={dismissNotification}
							class="hover:bg-opacity-10 inline-flex rounded-md p-1.5 transition-colors hover:bg-black focus:ring-2 focus:ring-current focus:ring-offset-2 focus:ring-offset-transparent focus:outline-none"
							aria-label="Dismiss notification">
							<span class="text-lg leading-none" aria-hidden="true">×</span>
						</button>
					</div>
				{/if}
			</div>

			<!-- Confirm Dialog Buttons -->
			{#if currentNotification.type === 'confirm' && !isTyping}
				<div class="mt-4 flex justify-end space-x-3">
					<button
						onclick={() => handleConfirmResponse(false)}
						class="rounded-md border border-gray-300 bg-white px-3 py-2 text-sm font-medium text-gray-700 transition-colors hover:bg-gray-50 focus:ring-2 focus:ring-purple-500 focus:ring-offset-2 focus:outline-none">
						{currentNotification.cancelText || 'Cancel'}
					</button>
					<button
						onclick={() => handleConfirmResponse(true)}
						class="rounded-md border border-transparent bg-purple-600 px-3 py-2 text-sm font-medium text-white transition-colors hover:bg-purple-700 focus:ring-2 focus:ring-purple-500 focus:ring-offset-2 focus:outline-none">
						{currentNotification.confirmText || 'Confirm'}
					</button>
				</div>
			{/if}

			<!-- Progress Bar (only for non-confirm notifications) -->
			{#if !isTyping && isVisible && currentNotification.type !== 'confirm'}
				<div class="bg-opacity-10 mt-2 h-1 w-full rounded-full bg-black">
					<div
						class="h-1 rounded-full bg-current transition-all ease-linear"
						style="width: 100%; animation: shrink {DISPLAY_DURATION}ms linear forwards;">
					</div>
				</div>
			{/if}
		</div>
	</div>
{/if}

<!-- Queue Indicator -->
{#if notifications.length > 0}
	<div class="fixed top-4 left-4 z-40">
		<div class="rounded-lg bg-gray-800 px-3 py-2 text-white shadow-lg">
			<span class="text-sm font-medium">
				{notifications.length} more notification{notifications.length === 1
					? ''
					: 's'}
			</span>
		</div>
	</div>
{/if}

<style>
	@keyframes shrink {
		from {
			width: 100%;
		}
		to {
			width: 0%;
		}
	}
</style>
```