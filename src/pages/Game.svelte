<script lang="ts">
    import type { CurrentRoute } from "svelte-router-spa/types/components/route";
    //export let gameId: number;
    export let currentRoute: CurrentRoute;

    let currentGameId = currentRoute.namedParams.id;
    let secretWord : string;
    let tries = 0;
    let turns = 0;
    let allGuessedLetters : string;
    let correctGuessedLetters : string;
    let wrongGuessedLetters : string;
    let gameStatus : number;
    let solutionString : string;
    let guessedLetter : string;
    let gameProgress : string;
    let currentGameInstance: {}

    let UpdateSolutionString = (sw: string, cgl: string[]) => {
        let result : string[] = [];
        for(let i = 0; i < sw.length; i++){
            if(cgl.includes(sw[i])){
                result.push(sw[i]);
            } else {
                result.push('*');
            }
        }
        return result.join(" ");
    }

    // let UpdateGuessedLettersString = (letters: []) => {
    //     return letters.join(", ");
    // }

    let updateGameStatus = (status: number) => {
        if(status == 0){
            return "In Progress"
        } else if (status == 1){
            window.alert("Game is lost!")
            return "Lost"
        } else if (status == 2){
            window.alert("Game is won!")
            return "Won"
        } else {
            return "No valid game"
        }
    }

    fetch(`https://localhost:7142/api/game?id=${currentGameId}`, {
			method: 'GET',
			headers: {
				'Content-Type': 'application/json'
			},
		})
        .then((x) => x.json())
			.then((game) => {
                game = game;
				secretWord = game.secretWord;
                tries = game.triesLeft;
                turns = game.turns;
                gameStatus = game.status;
                currentGameInstance = game;
                console.log(currentGameId);
                console.log(currentGameInstance);
                solutionString = UpdateSolutionString(game.secretWord, game.correctGuessedLetters);
                gameProgress = updateGameStatus(game.status);
			})

    let guess = () => {
        fetch(`https://localhost:7142/api/game/guess`, {
            method: 'POST',
            headers: {
				'Content-Type': 'application/json'
			},
			body: JSON.stringify({ "gameId":currentGameId, "guessedLetter":guessedLetter })
        })
        .then((x) => x.json())
        .then((game) => {
            solutionString = UpdateSolutionString(game.secretWord, game.correctGuessedLetters);
            gameProgress = updateGameStatus(game.status);
            allGuessedLetters = game.allGuessedLetters;
            correctGuessedLetters = game.correctGuessedLetters;
            wrongGuessedLetters = game.wrongGuessedLetters;
            tries = game.triesLeft;
            turns = game.turns;
            currentGameInstance = game;
        });
    }

    
        
</script>
    
<main>
    Game met id: {currentGameId}
    <br>
    Secret word: {solutionString}
    <br>
    Tries left: {tries}
    <br>
    Turns: {turns}
    <br>
    All Guessed letters: {allGuessedLetters}
    <br>
    Correct Guessed letters: {correctGuessedLetters}
    <br>
    Wrong Guessed letters: {wrongGuessedLetters}
    <br>
    Game is: {gameProgress}

    <form on:submit|preventDefault={guess}>
		<label for="input-guess">Gok een letter:</label>
		<input id="input-guess" type="string" bind:value={guessedLetter} />
		<button>Submit!</button>
	</form>
</main>