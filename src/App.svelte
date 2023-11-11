<script>
  import { is_client } from "svelte/internal";
  import Tictactoe from "./Tictactoe.svelte";
  import Title from "./Title.svelte";

  //   export let name;

  let board = Array(9); //.fill(TicTacToe); // Representa o estado do tabuleiro
  let childStates = Array(board.length).fill(false);
  let currentPlayer = "X"; // Inicia com o jogador X
  let whereNext = "";
  let isActive = true;
  /**
   * @type {string | null}
   */
  let winner = ""; // Armazena o vencedor (X, O, ou null)

  // Função whereNext
  /**
   * @param {{ detail: any; }} event
   */
  function define_whereNext(event) {
    console.log("called");
    if (
      document
        .querySelector(`.mega-tictactoe-grid > .border-${event.detail}`)
        .classList.contains("completed")
    ) {
      whereNext = "not-completed";
      console.log(whereNext);
    } else {
      whereNext = event.detail;
    }

    currentPlayer = currentPlayer === "X" ? "O" : "X";
  }
  /**
   * @param {{toString: () => string;}} elementIndex
   * @param {string} WN_Value
   */
  function define_isActive(WN_Value, elementIndex) {
    if (WN_Value == "not-completed") {
      if (
        document
          .querySelector(`.mega-tictactoe-grid > .border-${elementIndex}`)
          .classList.contains("completed")
      ) {
        isActive = false;
        return isActive;
      } else {
        isActive = true;
        return isActive;
      }
    } else if (WN_Value != "") {
      elementIndex == WN_Value ? (isActive = true) : (isActive = false);
      return isActive;
    } else if (WN_Value == "") {
      isActive = true;
      return isActive;
    }
  }

  function gameCompleted(event) {
    childStates[event.detail] = true;

    checkWinner();
  }

  function checkWinner() {
    const winningCombinations = [
      [0, 1, 2],
      [3, 4, 5],
      [6, 7, 8], // Linhas
      [0, 3, 6],
      [1, 4, 7],
      [2, 5, 8], // Colunas
      [0, 4, 8],
      [2, 4, 6], // Diagonais
    ];

    for (let combination of winningCombinations) {
      const [a, b, c] = combination;
      if (
        childStates[a] &&
        childStates[a] === childStates[b] &&
        childStates[a] === childStates[c]
      ) {
        winner = childStates[a]; // Definir o vencedor

        let topAttribute = "";
        let rightAttribute = "";
        let minWidthAttribute = "750px";
        let rotateAttribute = "rotate(0deg)";

        if (combination == winningCombinations[0]) {
          topAttribute = "22%";
        }
        if (combination == winningCombinations[1]) {
          topAttribute = "52%";
        }
        if (combination == winningCombinations[2]) {
          topAttribute = "83%";
        }

        if (combination == winningCombinations[3]) {
          topAttribute = "52%";
          rightAttribute = "34.5%";
          minWidthAttribute = "900px";
          rotateAttribute = "rotate(90deg)";
        }

        if (combination == winningCombinations[4]) {
          topAttribute = "52%";
          rightAttribute = "16%";
          minWidthAttribute = "900px";
          rotateAttribute = "rotate(90deg)";
        }
        if (combination == winningCombinations[5]) {
          topAttribute = "52%";
          rightAttribute = "-2%";
          minWidthAttribute = "900px";
          rotateAttribute = "rotate(90deg)";
        }

        let winLine = document.createElement("hr");
        winLine.classList.add("winLine");
        winLine.style.color = "greenyellow";
        winLine.style.backgroundColor = "greenyellow";
        winLine.style.position = "absolute";
        winLine.style.height = "5px";
        winLine.style.minWidth = minWidthAttribute;
        winLine.style.top = topAttribute;
        winLine.style.right = rightAttribute;
        winLine.style.transform = rotateAttribute;
        document.querySelector(".mega-tictactoe-grid").appendChild(winLine);
        return;
      }
    }

    // Verifica se o tabuleiro está cheio (empate)
    if (!childStates.includes("")) {
      winner = "Empate";
    }
  }

  // Função para reiniciar o jogo
  function resetGame() {
    board = Array(9).fill("");
    currentPlayer = "X";
    winner = "";
  }
</script>

<main>
  <Title />
  <div class="mega-game">
    <div class="mega-tictactoe-grid">
      {#each childStates as state, index}
        <div
          class:completed={state}
          class="{define_isActive(whereNext, index.toString())
            ? 'active'
            : 'inactive'} border-{index}"
        >
          <Tictactoe
            {currentPlayer}
            on:whereNext_Updated={define_whereNext}
            megaIndex={index}
            on:gameCompleted={gameCompleted}
          />
        </div>
      {/each}
    </div>
    <span style="position: absolute; right: 5%;">
      é a vez de <p
        style="font-size: 35px; font-weight: 700; margin: 0; color: {currentPlayer ==
        'X'
          ? '#0070ff'
          : '#ff3c3c'}"
      >
        {currentPlayer}
      </p>
    </span>
    {#if winner}
      <p>{winner === "Empate" ? "Empate!" : `Vencedor: ${winner}`}</p>
      <button on:click={resetGame}>Reiniciar</button>
    {/if}
  </div>
  <!-- <h1>Hello {name}!</h1>
  <p>
    Visit the <a href="https://svelte.dev/tutorial">Svelte tutorial</a> to learn
    how to build Svelte apps.
  </p> -->
</main>

<style>
  main {
    text-align: center;
    /* padding: 1em; */
    max-width: 240px;
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    justify-content: center;
  }

  .mega-game {
    display: flex;
    flex-direction: column;
    align-items: center;
    color: white;
  }

  .mega-tictactoe-grid {
    display: grid;
    grid-template: 33% 34% 33% / 33% 34% 33%;
  }

  .mega-tictactoe-grid > div {
    padding: 30px;
  }

  main > div > div > div.border-3,
  main > div > div > div.border-4,
  main > div > div > div.border-5,
  main > div > div > div.border-6,
  main > div > div > div.border-7,
  main > div > div > div.border-8 {
    border-top: 4px solid #fff;
  }

  main > div > div > div.border-0,
  main > div > div > div.border-1,
  main > div > div > div.border-3,
  main > div > div > div.border-4,
  main > div > div > div.border-6,
  main > div > div > div.border-7 {
    border-right: 4px solid #fff;
  }

  main > div > div > div.border-0,
  main > div > div > div.border-1,
  main > div > div > div.border-2,
  main > div > div > div.border-3,
  main > div > div > div.border-4,
  main > div > div > div.border-5 {
    border-bottom: 4px solid #fff;
  }

  main > div > div > div.border-1,
  main > div > div > div.border-2,
  main > div > div > div.border-4,
  main > div > div > div.border-5,
  main > div > div > div.border-7,
  main > div > div > div.border-8 {
    border-left: 4px solid #fff;
  }

  .inactive.border-3,
  .inactive.border-4,
  .inactive.border-5,
  .inactive.border-6,
  .inactive.border-7,
  .inactive.border-8 {
    border-top: 4px solid rgba(255, 255, 255, 0.12);
  }

  .inactive.border-0,
  .inactive.border-1,
  .inactive.border-3,
  .inactive.border-4,
  .inactive.border-6,
  .inactive.border-7 {
    border-right: 4px solid rgba(255, 255, 255, 0.12);
  }

  .inactive.border-0,
  .inactive.border-1,
  .inactive.border-2,
  .inactive.border-3,
  .inactive.border-4,
  .inactive.border-5 {
    border-bottom: 4px solid rgba(255, 255, 255, 0.12);
  }

  .inactive.border-1,
  .inactive.border-2,
  .inactive.border-4,
  .inactive.border-5,
  .inactive.border-7,
  .inactive.border-8 {
    border-left: 4px solid rgba(255, 255, 255, 0.12);
  }

  /* .active {
    background-color: red;
  } */

  .inactive {
    pointer-events: none;
    /* opacity: 0.25; */
  }

  .winLine {
    color: greenyellow;
  }

  /* h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 4em;
    font-weight: 100;
  } */

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>
