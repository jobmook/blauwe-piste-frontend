<script lang="ts">
	import { navigateTo } from 'svelte-router-spa';

	let playerName: string;
	let gameID: number;
	export let newGame : object;

	const start = () => {
		console.log('playerName:', playerName);

		// nadelen van fetch() zo gebruiken:
		// - veel handmatig werk met content-types, stringify/parse (.json())
		// - niet getyped
		// - zelf isLoading-variabelen bijhouden (en testen!)
		// - niks geregeld voor herbruiken van requests
		// - geen globale errorhandler
		// - geen intercepties

		// library? svelte-query? axios?
		fetch('https://localhost:7142/api/game', {
			method: 'POST',
			headers: {
				'Content-Type': 'application/json'
			},
			body: JSON.stringify({ playerName })
		})
			.then((x) => x.json())
			.then((game) => {
				newGame = game;
				gameID = game.gameID;
				navigateTo(`/game/${gameID}`);
			});
	};

</script>

<main>
	<h1>Nieuw spel</h1>

	<form on:submit|preventDefault={start}>
		<label for="input-name">Je naam?</label>
		<input id="input-name" bind:value={playerName} />
		<button>Start!</button>
	</form>

	Game ID: {gameID}
</main>