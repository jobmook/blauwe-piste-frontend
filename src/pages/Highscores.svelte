<script lang="ts">
    let gamesList : {}[] = [];
    let bestRatio: {} = {};
    let bestMostGuessed: {} = {};
    

    fetch("https://localhost:7142/api/stats/statistics", {
        method: 'GET',
        headers: {
				'Content-Type': 'application/json'
			}
    })
    .then((x) => x.json())
    .then((listOfGames) => {
        gamesList = listOfGames;
        // listOfGames.forEach(element => {
        // let statsElement = {"GameID" : element.gameID, "Fouten": element.wrongGuessedLetters.length, "Woord": element.secretWord, "Progressie":element.status}
        // gamesList.push(statsElement)});
    });

    fetch("https://localhost:7142/api/stats/highscores/best-most-guessed", {
        method: 'GET',
        headers: {
				'Content-Type': 'application/json'
			}
    })
    .then((x) => x.json())
    .then((player) => {
        bestMostGuessed = player;
        let bestMostGessedName = player.name;
    });

    fetch("https://localhost:7142/api/stats/highscores/best-ratio", {
        method: 'GET',
        headers: {
				'Content-Type': 'application/json'
			}
    })
    .then((x) => x.json())
    .then((player) => {
        bestRatio = player;
    });


</script>

<main>
<table>
    <thead>
        <!-- svelte-ignore empty-block -->
        <th>GameID</th>
        <th>Fouten</th>
        <th>Woord</th>
        <th>Progressie</th>
    </thead>
    <tbody>
        {#each gamesList as item}
        <tr>
            <td>{item["gameID"]}</td>
            <td>{item["wrongGuessedLetters"].length}</td>
            <td>{item["secretWord"]}</td>
            <td>{item["status"]}</td>
        </tr>
        {/each}
    </tbody>
</table>

<table>
    <thead>
        <th>Speler</th>
        <th>Aantal woorden goed</th>
    </thead>
    <tbody>
        <tr>
            <td>{bestMostGuessed["name"]}</td>
            <td>{bestMostGuessed["winCount"]}</td>
        </tr>
    </tbody>
</table>

<table>
    <thead>
        <th>Speler</th>
        <th>Ratio</th>
    </thead>
    <tbody>
        <tr>
            <td>{bestRatio["name"]}</td>
            <td>{bestRatio["ratio"]}</td>
        </tr>
    </tbody>
</table>
</main>