<script>
	import { onMount } from "svelte";
	let apiData = {
		song: {
			track: "/dev/urandom",
			coverUrl: "favicon.png",
		},
		artist: {
			frontFacingName: "the Linux kernel",
			url: "https://wav.blue/",
		},
		player: {
			percentage: 0,
			isPlaying: true,
		},
	};
	let apiDataMisc = "[ Updating Status ]";
	let apiDataSteam = {
		status: {
			stateName: "[ Updating Status ]",
			game: null,
		},
	}
	let apiDataTime = {
		formattedTime: "?",
		formattedUSTime: "?",
		epoch: 0,
	};
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
	let loaded = {
		spotify: false,
		pc: false,
		steam: false,
		time: false,
	}
	
	function getApiData() {
		loaded.spotify = false;
		fetch("https://api.wav.blue/spotify")
			.then(response => response.json())
			.then(data => {
				console.log(data);
				const code = data?.code;
				if (code === 0) apiData = data;
				else {
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
				loaded.spotify = true;
			}).catch(error => {
				console.error(error);
				return [];
			});
	}
	function getApiDataForSteam() {
		loaded.steam = false;
		fetch("https://api.wav.blue/steam")
			.then(response => response.json())
			.then(data => {
				console.log(data);
				const code = data?.code;
				if (code === 1) apiDataSteam = data;
				else {
					apiDataSteam = {
						status: {
							stateName: "Unknown",
							game: null,
						},
					}
				}
				loaded.steam = true;
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
				loaded.pc = true;
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
			getApiDataForSteam();
			setInterval(() => { getApiData(); }, 6000);
			setInterval(() => { getApiDataForSteam(); }, 15000);

			fetch("https://api.wav.blue/currenttime")
				.then(response => response.json())
				.then(data => {
					apiDataTime = {
						formattedTime: data.time,
						formattedUSTime: new Date(data.timeEpoch).toLocaleTimeString("en-US"),
						epoch: data.timeEpoch,
					};
					loaded.time = true;
					setInterval(() => {
						let newTime = apiDataTime.epoch + 1000;
						apiDataTime = {
							formattedTime: new Date(newTime).toLocaleTimeString("en-GB"),
							formattedUSTime: new Date(newTime).toLocaleTimeString("en-US"),
							epoch: newTime,
						};
						console.log(apiDataTime);
					}, 1000);
				}).catch(error => {
					console.error(error);
					return [];
				});
		} catch (error) {
			console.warn("Failed to fetch API data, server might be down - Check https://status.wav.blue/ for service status")
			console.warn(error);
		}
		
		/*setInterval(() => {
			/**
			* @type {string | null}
			*
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
		}, 5850);*/
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
	<img src="favicon.png" alt="FATAL Logo" width="175rem" height="175rem">

	<!--<h1 style="margin-top: 0px; margin-bottom: 0px;">
		Hi, I'm <span class="gradtext">{loopedText}</span>!
		<span style="font-size: x-small;">(&gtω&lt)</span>
	</h1>
	<span style="font-size: medium;">
		Self-taught full-stack programmer and sometimes graphic designer
	</span>-->
	<br><br>
	<button class="badgebtn shiny">FREE CRAP HERE!</button>
	<button class="badgebtn">GITHUB</button>
	<button class="badgebtn">GITHUB</button>
	<button class="badgebtn">GITHUB</button>
	<br><br>
	<button class="badgebtn warning" on:click={thefunny}><img class="icon" src="/icons/warning.svg" alt="Icon">ARM NUCLEAR WARHEADS</button>
	<br><br><br><br>

	<div class="card music {loaded.spotify === false ? "loadingshiny" : ""}">
		<img class="icon" src="/icons/music.svg" alt="Music icon">
		<div>
			<span class="cardtext">{#if apiData.song.track == "a ad" && apiData.song.coverUrl == "favicon.png"}Stuck listening to{:else if apiData.player.isPlaying}Listening to{:else}Paused on{/if} <b><code class="desc">{apiData.song.track}</code></b><br>By <b><code class="desc">{apiData.artist.frontFacingName}</code></b></span>
			<br>
			<center>
				<img src="{apiData.song.coverUrl}" alt="Album cover" class="coverart">
			</center>
		</div>
	</div>
	<br>
	<div class="card {loaded.steam === false ? "loadingshiny" : ""}">
		<img class="icon" src="/icons/steam.svg" alt="Steam icon">
		<div>
			<span class="cardtext"><b><code class="desc">{#if apiDataSteam.status.game}{apiDataSteam.status.stateName} {apiDataSteam.status.game}{:else}{apiDataSteam.status.stateName}{/if}</code></b> on Steam</span>
		</div>
	</div>
	<br>
	<div class="card {loaded.pc === false ? "loadingshiny" : ""}">
		<img class="icon" src="/icons/pc.svg" alt="Computer icon">
		<div>
			<span class="cardtext">My desktop is currently <b><code class="desc">{apiDataMisc}</code></b></span>
			<!--<button class="badgebtn warning" style="margin-left: 0px; margin-top: 0.15rem;" on:click={refreshDesktopStatus}>REFRESH</button>-->
		</div>
	</div>
	<br>
	<div class="card {loaded.time === false ? "loadingshiny" : ""}">
		<img class="icon" src="/icons/time.svg" alt="Clock icon">
		<div>
			<span class="cardtext">It is currently <b><code class="desc">{apiDataTime.formattedTime} ({apiDataTime.formattedUSTime})</code></b> for me</span>
		</div>
	</div>
	<br>
</div>
