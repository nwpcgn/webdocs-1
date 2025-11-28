---
id: VideoScreenshots
title: Screenshots 1 
sidebar_label: Screenshots 1
sidebar_position: 1011
---


# Notification Component



## Main Component

```html title='VideoScreenshoTool.svelte'

<script>
    import {
        onMount
    } from 'svelte';
    // State variables
    let videoFile = $state(null);
    let videoElement = $state();
    let videoUrl = $state('');
    let canvasElement;
    let ctx;
    let screenshotGallery = $state([]);
    let isAutoCapturing = $state(false);
    let captureInterval = $state(1000); // 1 second by default
    let intervalId = null;
    let videoLoaded = $state(false);
    // Handle file upload
    function handleFileUpload(event) {
        const file = event.target.files[0];
        if (file && file.type.includes('video/')) {
            videoFile = file;
            videoUrl = URL.createObjectURL(file);
            videoLoaded = false;
        }
    }
    // Take a screenshot from the current video frame
    function takeScreenshot() {
        if (!videoElement || videoElement.paused || videoElement.ended) return;
        // Set canvas dimensions to match video
        canvasElement.width = videoElement.videoWidth;
        canvasElement.height = videoElement.videoHeight;
        // Draw the current video frame to the canvas
        ctx.drawImage(videoElement, 0, 0, canvasElement.width, canvasElement.height);
        // Convert canvas to image data URL
        const imageDataUrl = canvasElement.toDataURL('image/png');
        // Add to gallery
        screenshotGallery = [...screenshotGallery, {
            id: Date.now(),
            src: imageDataUrl,
            timestamp: formatTime(videoElement.currentTime)
        }];
    }
    // Format time in seconds to MM:SS format
    function formatTime(seconds) {
        const minutes = Math.floor(seconds / 60);
        const remainingSeconds = Math.floor(seconds % 60);
        return `${minutes.toString().padStart(2, '0')}:${remainingSeconds.toString().padStart(2, '0')}`;
    }
    // Format bytes to human-readable size
    function formatFileSize(bytes) {
        if (bytes === 0) return '0 Bytes';
        const k = 1024;
        const sizes = ['Bytes', 'KB', 'MB', 'GB', 'TB'];
        const i = Math.floor(Math.log(bytes) / Math.log(k));
        return parseFloat((bytes / Math.pow(k, i)).toFixed(2)) + ' ' + sizes[i];
    }
    // Calculate megabytes from bytes
    function bytesToMB(bytes) {
        return (bytes / (1024 * 1024)).toFixed(2);
    }
    // Toggle automatic screenshot capture
    function toggleAutoCapture() {
        if (isAutoCapturing) {
            clearInterval(intervalId);
            intervalId = null;
        } else {
            intervalId = setInterval(takeScreenshot, captureInterval);
        }
        isAutoCapturing = !isAutoCapturing;
    }
    // Update capture interval
    function updateCaptureInterval(event) {
        captureInterval = event.target.value * 1000; // Convert to milliseconds
        if (isAutoCapturing) {
            clearInterval(intervalId);
            intervalId = setInterval(takeScreenshot, captureInterval);
        }
    }
    // Download a screenshot
    function downloadScreenshot(imageDataUrl, timestamp) {
        const a = document.createElement('a');
        a.href = imageDataUrl;
        a.download = `screenshot-${timestamp.replace(':', '-')}.png`;
        document.body.appendChild(a);
        a.click();
        document.body.removeChild(a);
    }
    // Clear all screenshots
    function clearScreenshots() {
        screenshotGallery = [];
    }
    // Handle video loaded
    function handleVideoLoaded() {
        videoLoaded = true;
    }
    // Clean up on component unmount
    onMount(() => {
        canvasElement = document.createElement('canvas');
        ctx = canvasElement.getContext('2d');
        return () => {
            if (videoUrl) URL.revokeObjectURL(videoUrl);
            if (intervalId) clearInterval(intervalId);
        };
    });
</script>
<main class="container mx-auto p-4">
    <h1 class="text-3xl font-bold mb-6">Video Screenshot Tool</h1>
    <div class="mb-6">
        <label for="video-upload" class="block mb-2 font-medium"> Upload Video File </label>
        <input id="video-upload" type="file" accept="video/*" onchange={handleFileUpload} class="block w-full text-sm text-gray-900 border border-gray-300 rounded-lg cursor-pointer bg-gray-50 p-2.5" />
    </div> {#if videoFile}
    <p class="mt-2 text-sm text-gray-600">
      File: {videoFile.name} ({bytesToMB(videoFile.size)} MB)
    </p>
  {/if}
  
  {#if videoUrl}
    <div class="mb-6">
      <div class="relative bg-black rounded-lg overflow-hidden">
        <video 
          bind:this={videoElement}
          src={videoUrl}
          controls
          onloadeddata={handleVideoLoaded}
          class="w-full max-h-[60vh] mx-auto"
        >
          Your browser does not support the video tag.
        </video>
      </div>
      
      {#if videoLoaded}
        <div class="mt-4 flex flex-wrap gap-4">
          <button 
            onclick={takeScreenshot}
            class="px-4 py-2 bg-green-600 text-white rounded-lg hover:bg-green-700 transition-colors"
          >
            Take Screenshot
          </button>
          
          <button 
            onclick={toggleAutoCapture}
            class={`px-4 py-2 ${isAutoCapturing ? 'bg-red-600 hover:bg-red-700' : 'bg-blue-600 hover:bg-blue-700'} text-white rounded-lg transition-colors`}
          >
            {isAutoCapturing ? 'Stop Auto Capture' : 'Start Auto Capture'}
          </button>
          
          {#if screenshotGallery.length > 0}
            <button 
              onclick={clearScreenshots}
              class="px-4 py-2 bg-gray-600 text-white rounded-lg hover:bg-gray-700 transition-colors"
            >
              Clear All Screenshots
            </button>
          {/if}
        </div>
        
        <div class="mt-4">
          <label for="interval" class="block mb-2 font-medium">
            Auto Capture Interval (seconds): {captureInterval / 1000}
          </label>
          <input 
            id="interval"
            type="range" 
            min="0.1" 
            max="10" 
            step="0.1" 
            value={captureInterval / 1000} 
            oninput={updateCaptureInterval}
            class="w-full h-2 bg-gray-200 rounded-lg appearance-none cursor-pointer"
          />
        </div>
      {/if}
    </div>
    
    {#if screenshotGallery.length > 0}
      <div class="mt-8">
        <h2 class="text-2xl font-bold mb-4">Screenshots ({screenshotGallery.length})</h2>
        <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4">
          {#each screenshotGallery as screenshot (screenshot.id)}
            <div class="border rounded-lg overflow-hidden">
              <img src={screenshot.src || "/placeholder.svg"} alt={`Screenshot at ${screenshot.timestamp}`} class="w-full h-auto" />
              <div class="p-2 flex justify-between items-center bg-gray-100">
                <span class="text-sm font-medium">{screenshot.timestamp}</span>
                <button 
                  onclick={() => downloadScreenshot(screenshot.src, screenshot.timestamp)}
                  class="text-blue-600 hover:text-blue-800"
                >
                  Download
                </button>
              </div>
            </div>
          {/each}
        </div>
      </div>
    {/if}
  {:else}
    <div class="bg-gray-100 p-8 rounded-lg text-center">
      <p class="text-lg">Upload a video file to get started</p>
    </div>
  {/if}
</main>

<style>
  :global(body) {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen,
      Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  }
</style>
```