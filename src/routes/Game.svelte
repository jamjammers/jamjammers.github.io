<script lang="ts">
  import { onMount } from 'svelte';
  // import { createUnityInstance } from '$lib/Builds.loader.js';
  let canvas:any;
  onMount(() => {
      var buildUrl = "Build";
      var config = {
        arguments: [],
        dataUrl: buildUrl + "/Builds.data",
        frameworkUrl: buildUrl + "/Builds.framework.js",
        codeUrl: buildUrl + "/Builds.wasm",
        streamingAssetsUrl: "StreamingAssets",
        companyName: "DefaultCompany",
        productName: "AnalysisRhythmGame",
        productVersion: "0.1.0"
      }
    // The Unity loader attaches `createUnityInstance` to the window in classic script mode.
    // createUnityInstance(canvas, config);
  });
</script>
<div>
  <div class="game-container">
    <canvas class="canvas" bind:this={canvas}></canvas>
  </div>
</div>
<style>
  .game-container {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100vw;
    height: calc(100vw / 2);
    position: relative;
    :global(#unity-canvas) {
      position: absolute;
      top:0;
      left:0;
      transform: scale(calc(100vw / 1280px));
    }
  }
		@media (min-aspect-ratio: 2) {
			.game-container {
				width: calc(100vh *2);
				height: 100vh;
        
				:global(#unity-canvas) {
          position: absolute;
          top:0;
          left:0;
					transform: scale(calc(100vh / 640px));
				}
			}
		}
</style>