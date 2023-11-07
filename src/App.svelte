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
      console.log(isActive, WN_Value, elementIndex);
      return isActive;
    } else if (WN_Value == "") {
      isActive = true;
      console.log(WN_Value);
      return isActive;
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
      {#each board as ticGame, index}
        <div
          class="{define_isActive(whereNext, index.toString())
            ? 'active'
            : 'inactive'} border-{index}"
        >
          <Tictactoe
            {currentPlayer}
            on:whereNext_Updated={define_whereNext}
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
