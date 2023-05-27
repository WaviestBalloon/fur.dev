<script>
	import { onMount } from "svelte";
	let apiData = {
		song: {
			track: "nothing",
			coverUrl: "favicon.png",
		},
		artist: {
			frontFacingName: "block.minecraft.air",
			url: "https://wav.blue/",
		},
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
				const code = data?.code;
				if (code === 0) apiData = data;
				else {
					console.log("Unexpected API response");
					switch (code) {
						case 5:
							apiData = {
								song: {
									track: "a ad",
									coverUrl: "favicon.png",
								},
								artist: {
									frontFacingName: "Spotify",
									url: "https://wav.blue/",
								},
								player: {
									percentage: 0,
									isPlaying: true,
								},
							};
							break;
						default:
							console.log("Res API code: " + code)
							apiData = {
								song: {
									track: "nothing",
									coverUrl: "favicon.png",
								},
								artist: {
									frontFacingName: "block.minecraft.air",
									url: "https://wav.blue/",
								},
								player: {
									percentage: 0,
									isPlaying: true,
								},
							};
							break;
					}
				}
			}).catch(error => {
				console.error(error);
				return [];
			});
	}
	function refreshDesktopStatus() {
		fetch("https://api.wav.blue/pc")
			.then(response => response.json())
			.then(data => {
				apiDataMisc = data?.message;
			}).catch(error => {
				console.error(error);
				return [];
			});
	}
	let stupid = [
		"ihatethishackercrap.mp3",
		"hno1.mp3",
		"hno3.mp3",
		"noalso.mp3",
		"mno1.mp3",
		"mno3.mp3",
		"buzzer.mp3",
		"no.mp3",
		"disc.mp3",
		"death.mp3",
		"bye.mp3",
		"anger.mp3",
		"portal2buzzer.mp3",
	];
	function thefunny() { new Audio(`sounds/${stupid[Math.floor(Math.random() * stupid.length)]}`).play(); }

	onMount(async () => {
		try { // try catch to prevent the page from breaking if the API is down
			getApiData();
			refreshDesktopStatus();
			setInterval(() => { getApiData(); }, 6000);
		} catch (error) {
			console.warn("Failed to fetch API data, server might be down - Check https://status.wav.blue/ for service status")
			console.warn(error);
		}
		
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
<span class="shadowoverlay"></span>
<span class="screenoverlay">
	<img src="https://fur.dev/assets/corner.svg" alt="Corner" class="left cornerboilerplate" width="35rem">
	<img src="https://fur.dev/assets/corner.svg" alt="Corner" class="right cornerboilerplate" width="35rem">
	<img src="https://fur.dev/assets/corner.svg" alt="Corner" class="bottomleft cornerboilerplate" width="35rem">
	<img src="https://fur.dev/assets/corner.svg" alt="Corner" class="bottomright cornerboilerplate" width="35rem">
</span>

<div class="mainheader">
	<img src="favicon.png"width="175rem" height="175rem">

	<h1 style="margin-top: 0px; margin-bottom: 0px;">
		Hi, I'm <span class="gradtext">{loopedText}</span>!
		<span style="font-size: x-small;">(&gtω&lt)</span>
	</h1>
	<span style="font-size: medium;">
		Self-taught full-stack programmer and sometimes graphic designer
	</span>
	<br><br>
	<button class="badgebtn shiny">FREE CRACK HERE!</button>
	<button class="badgebtn">GITHUB</button>
	<button class="badgebtn">GITHUB</button>
	<button class="badgebtn">GITHUB</button>
	<br><br>
	<button class="badgebtn warning" on:click={thefunny}><img class="icon" src="/icons/warning.svg" alt="Icon">ARM WARHEAD</button>
	<br><br><br><br>

	<div class="card music">
		<img class="icon" src="/icons/music.svg" alt="Music icon">
		<div>
			<span class="cardtext">{#if apiData.song.track == "a ad" && apiData.song.coverUrl == "favicon.png"}Stuck listening to{:else if apiData.player.isPlaying}Listening to{:else}Paused on{/if} <b><code class="musicdesc">{apiData.song.track}</code></b><br>By <b><code class="musicdesc">{apiData.artist.frontFacingName}</code></b></span>
			<br>
			<center>
				<img src="{apiData.song.coverUrl}" alt="Album cover" class="coverart">
			</center>
		</div>
	</div>
	<br>
	<div class="card">
		<img class="icon" src="/icons/pc.svg" alt="Computer icon">
		<div>
			<span class="cardtext">My desktop is currently <b><code class="musicdesc">{apiDataMisc}</code></b></span>
			<br>
			<!--<button class="badgebtn warning" style="margin-left: 0px; margin-top: 0.15rem;" on:click={refreshDesktopStatus}>REFRESH</button>-->
		</div>
	</div>
	<br>
</div>
