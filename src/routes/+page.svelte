<script>
	import { onMount } from "svelte";
	let apiData = {
		// @ts-ignore
		song: {
			track: "nothing",
			coverUrl: "favicon.png",
		},
		artist: {
			frontFacingName: "air_block",
			url: "https://wav.blue/",
		},
		// @ts-ignore
		player: {
			percentage: 0,
			isPlaying: true,
		},
	};
	let apiDataMisc = "not responding";
	let loopedText = "Waviest";
	let possibleNames = [
		"Waviest",
		"Wavi",
		"Wave",
		"Wavey",
		"Wavy",
		"Wav",
		"WaviestBalloon",
	];
	
	function getApiData() {
		fetch("https://api.wav.blue/spotify")
			.then(response => response.json())
			.then(data => {
				console.log(data);
				if (!data?.error) apiData = data;
				else {
					console.log("Unexpected API response");
					apiData = {
						// @ts-ignore
						song: {
							track: "nothing",
							coverUrl: "favicon.png",
						},
						artist: {
							frontFacingName: "block.minecraft.air",
							url: "https://wav.blue/",
						},
						// @ts-ignore
						player: {
							percentage: 0,
							isPlaying: true,
						},
					};
				}
			}).catch(error => {
				console.log(error);
				return [];
			});
	}
	function refreshDesktopStatus() {
		fetch("https://api.wav.blue/pc")
			.then(response => response.json())
			.then(data => {
				apiDataMisc = data?.message;
			}).catch(error => {
				console.log(error);
				apiDataMisc = "not responding to pings";
				return [];
			});
	}

	onMount(async () => {
		getApiData();
		refreshDesktopStatus();
		setInterval(() => {
			getApiData();
		}, 6000);
		setInterval(() => {
			/**
			* @type {string | null}
			*/
			let prev = null;
			let intervalId = setInterval(() => {
				if (loopedText.length >= 1) {
					loopedText = loopedText.slice(1);
				} else {
					setTimeout(() => {
						let newer = possibleNames[Math.floor(Math.random() * possibleNames.length)];
						if (newer === prev) {
							newer = possibleNames[Math.floor(Math.random() * possibleNames.length)];
						} else {
							loopedText = newer;
							prev = newer;
						}
					}, 500);
					clearInterval(intervalId);
				}
			}, 50);
		}, 5850);
	});
</script>
<span class="screenoverlay">
	<img src="https://fur.dev/assets/corner.svg" alt="Corner" class="left cornerboilerplate" width="35rem">
	<img src="https://fur.dev/assets/corner.svg" alt="Corner" class="right cornerboilerplate" width="35rem">
	<img src="https://fur.dev/assets/corner.svg" alt="Corner" class="bottomleft cornerboilerplate" width="35rem">
	<img src="https://fur.dev/assets/corner.svg" alt="Corner" class="bottomright cornerboilerplate" width="35rem">
</span>

<div class="mainheader">
	<h1>Hi, I'm <span class="gradtext">{loopedText}</span>! <span style="font-size: x-small;">(&gtω&lt)</span></h1>

	<div class="card music">
		<img class="icon" src="/icons/music.svg" alt="Music icon">
		<div>
			<span class="cardtext">{#if apiData.player.isPlaying}Listening to{:else}Paused on{/if} <b><code class="musicdesc">{apiData.song.track}</code></b>, by <b><code class="musicdesc">{apiData.artist.frontFacingName}</code></b>...</span>
			<br>
			<img src="{apiData.song.coverUrl}" alt="Album cover" class="coverart" width="125rem">
		</div>
	</div>
	<br>
	<div class="card">
		<img class="icon" src="/icons/pc.svg" alt="Computer icon">
		<div>
			<span class="cardtext">My desktop is currently <b><code class="musicdesc">{apiDataMisc}</code></b>...</span>
			<br>
			<button class="badgebtn warning" style="margin-left: 0px; margin-top: 0.15rem;" on:click={refreshDesktopStatus}>REFRESH</button>
		</div>
	</div>
	<br>
</div>
