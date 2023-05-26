<script>
	import { onMount } from "svelte";
	let apiData = {"artist":{"frontFacingName":"Carpenter Brut","url":"https://open.spotify.com/artist/1l2oLiukA9i5jEtIyNWIEP"},"song":{"album":"TRILOGY","track":"Looking For Tracy Tzu","coverUrl":"https://i.scdn.co/image/ab67616d0000b2731b1d6c550aaaae5acf220e84"},"player":{"isPlaying":true,"progress":{"hms":"00:01:39","ms":99974},"duration":{"hms":"00:04:17","ms":257614},"percentage":38.80767349600565}};

	function getApiData() {
		fetch("https://api.wav.blue/spotify")
			.then(response => response.json())
			.then(data => {
				console.log(data);
				if (!data?.error) apiData = data;
				else {
					console.log("API ERROR");
					apiData = {
						// @ts-ignore
						song: {
							track: "nothing",
							coverUrl: "favicon.png"
						},
						artist: {
							frontFacingName: "air_block",
							url: "https://wav.blue/"
						},
						// @ts-ignore
						player: {
							percentage: 0
						},
					};
				}
			}).catch(error => {
				console.log(error);
				return [];
			});
	}

	onMount(async () => {
		getApiData();
		setInterval(() => {
			getApiData();
		}, 6000);
	});
</script>

<!--<h1>> <span class="gradtext">WAVIEST</span></h1>-->

<center>
	<p>
		Currently listening to <b>{apiData.song.track}</b>, by <b>{apiData.artist.frontFacingName}</b>...
		<br>{#if apiData.player.isPlaying}
		<code>PLAYING</code>
	{:else}
		<code>PAUSED</code>
	{/if}<code> -> {Math.floor(apiData.player.percentage)}%</code>
		<br><br><img src={apiData.song.coverUrl} style="border-radius: 0.55rem;" alt="Album cover" width="200" height="200" />
		<br><br>
		<button class="badgebtn-warning" on:click={getApiData}>REFRESH</button>
	</p>
</center>
