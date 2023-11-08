<script>
  import Tictactoe from "./Tictactoe.svelte";
  import Title from "./Title.svelte";

  //   export let name;

  let board = Array(9); //.fill(TicTacToe); // Representa o estado do tabuleiro
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
    whereNext = event.detail;
    currentPlayer = currentPlayer === "X" ? "O" : "X";
  }
  /**
   * @param {{toString: () => string;}} elementIndex
   * @param {string} WN_Value
   */
  function define_isActive(WN_Value, elementIndex) {
    if (WN_Value != "") {
      elementIndex == WN_Value ? (isActive = true) : (isActive = false);
      return isActive;
    } else if (WN_Value == "") {
      isActive = true;
      return isActive;
    }
  }

  function gameCompleted(event) {
    document.querySelector(
      `.border-${event.detail}.svelte-1f4rfnf`
    ).classList += " completed";
    console.log("done");
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
      {#each board as ticGame, index}
        <div
          class="{define_isActive(whereNext, index.toString())
            ? 'active'
            : 'inactive'} border-{index}"
        >
          <Tictactoe
            {currentPlayer}
            on:whereNext_Updated={define_whereNext}
            on:gameCompleted={gameCompleted}
            megaIndex={index}
          />
        </div>
      {/each}
    </div>
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
    padding: 1em;
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
    grid-template: 32% 32% 32% / 32% 32% 32%;
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

  /* .active {
    background-color: red;
  } */

  .inactive {
    pointer-events: none;
    opacity: 0.25;
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
