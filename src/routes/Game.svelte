<script lang="ts">
  import { onMount } from 'svelte';

  let canvas:any;

  onMount(() => {
    const script = document.createElement('script');
    script.src = '/Build/Builds.loader.js';
    script.onload = () => {
      const config = {
        dataUrl: '/Build/Builds.data.br',
        frameworkUrl: '/Build/Builds.framework.js.br',
        codeUrl: '/Build/Builds.wasm.br',
        streamingAssetsUrl: 'StreamingAssets',
        companyName: 'DefaultCompany',
        productName: 'AnalysisRhythmGame',
        productVersion: '0.1.0',
      };
      createUnityInstance(canvas, config);
    };
    document.body.appendChild(script);
  });
</script>

<div class="game-container">
  <canvas bind:this={canvas} width="960" height="600"></canvas>
</div>

<style>
  .game-container {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 100vh;
  }
</style>