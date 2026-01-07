<script lang="ts">
	import { onMount } from 'svelte';
	import type { HtmlTagDescriptor } from 'vite';
	// import { createUnityInstance } from '$lib/Builds.loader.js';

	function unityShowBanner(msg: string, type: string) {
		var warningBanner: HTMLElement = document.querySelector('#unity-warning')!;
		if (warningBanner == null) return;
		function updateBannerVisibility() {
			warningBanner.style.display = warningBanner.children.length ? 'block' : 'none';
		}
		var div = document.createElement('div');
		div.innerHTML = msg;
		warningBanner.appendChild(div);
		if (type == 'error') div.style = 'background: red; padding: 10px;';
		else {
			if (type == 'warning') div.style = 'background: yellow; padding: 10px;';
			setTimeout(function () {
				warningBanner.removeChild(div);
				updateBannerVisibility();
			}, 5000);
		}
		updateBannerVisibility();
	}

	onMount(() => {
	  var canvas: HTMLCanvasElement = document.querySelector('#unity-canvas')!;

		var buildUrl = 'Build';
		var loaderUrl = buildUrl + '/WebGL.loader.js';
		var config = {
			arguments: [],
			dataUrl: buildUrl + '/WebGL.data',
			frameworkUrl: buildUrl + '/WebGL.framework.js',
			codeUrl: buildUrl + '/WebGL.wasm',
			streamingAssetsUrl: 'StreamingAssets',
			companyName: 'DefaultCompany',
			productName: 'AnalysisRhythmGame',
			productVersion: '0.1.0',
			showBanner: unityShowBanner
		};

		if (/iPhone|iPad|iPod|Android/i.test(navigator.userAgent)) {
			var meta = document.createElement('meta');
			meta.name = 'viewport';
			meta.content =
				'width=device-width, height=device-height, initial-scale=1.0, user-scalable=no, shrink-to-fit=yes';
			document.getElementsByTagName('head')[0].appendChild(meta);
			document.querySelector('#unity-container')!.className = 'unity-mobile';
			canvas.className = 'unity-mobile';
		} else {
      canvas.className = 'unity-desktop';
		}

		(document.querySelector('#unity-loading-bar')! as HTMLElement).style.display = 'block';
var script = document.createElement("script");
      script.src = loaderUrl;
      script.onload = () => {
        createUnityInstance(canvas, config, (progress:number) => {
          (document.querySelector("#unity-progress-bar-full")! as HTMLElement).style.width = 100 * progress + "%";
              }).then((unityInstance) => {

                (document.querySelector("#unity-loading-bar")! as HTMLElement).style.display = "none";
                (document.querySelector("#unity-fullscreen-button")! as HTMLElement).onclick = () => {
                  unityInstance.SetFullscreen(1);
                };

              }).catch((message) => {
                alert(message);
              });
            };

      document.body.appendChild(script);
          });
</script>

<div id="unity-container" class="unity-desktop">
	<canvas id="unity-canvas" width="1600" height="800" tabindex="-1"></canvas>
	<div id="unity-loading-bar">
		<div id="unity-logo"></div>
		<div id="unity-progress-bar-empty">
			<div id="unity-progress-bar-full"></div>
		</div>
	</div>
	<div id="unity-warning"></div>
	<div id="unity-footer">
		<div id="unity-logo-title-footer"></div>
		<div id="unity-fullscreen-button"></div>
		<div id="unity-build-title">AnalysisRhythmGame</div>
	</div>
</div>

<style>
	/* .unity-desktop {
		display: flex;
		justify-content: center;
		align-items: center;
		width: 100vw;
		height: calc(100vw / 2);
		position: relative;
		:global(#unity-canvas) {
			position: absolute;
			top: 0;
			left: 0;
			 transform: scale(calc(100vw / 1280px)); 
		}
	}
	@media (min-aspect-ratio: 2) {
		.unity-desktop {
			width: calc(100vh * 2);
			height: 100vh;

			:global(#unity-canvas) {
				position: absolute;
				top: 0;
				left: 0;
				 transform: scale(calc(100vh / 640px)); 
			}
		}
	} */
</style>
