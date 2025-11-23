<!--
    Easy mode     :  8 x 8   10 mines
    Advanced mode : 16 x 16  40 mines
    Pro mode      : 30 x 16  99 mines

    Colors
    Unclicked cells : rgb(211, 211, 211) (lightgray)
    Clicked cells   : rgb(123, 163, 239) (light blue)

-->

<script lang="ts">
  import Bird from "$lib/assets/svg-bird.svelte";
  import One from "$lib/assets/svg-one.svelte";
  import Two from "$lib/assets/svg-two.svelte";
  import Three from "$lib/assets/svg-three.svelte";
  import Four from "$lib/assets/svg-four.svelte";
  import Five from "$lib/assets/svg-five.svelte";
  import Six from "$lib/assets/svg-five.svelte";
  import Seven from "$lib/assets/svg-five.svelte";
  import Eight from "$lib/assets/svg-five.svelte";
  
  let fieldCols = 8;    // X-col axis
  let fieldRows = 12;   // Y-row axis
  let birdsTotal = 10;
  let adjacentBirds = 0;
  let birdLocations = CreateBirdLocations(fieldCols, fieldRows, birdsTotal);

  function ShowCell(x: number, y: number) {
    console.log(`You clicked cell X:${x} Y: ${y}`);
  }

  function CreateBirdLocations(maxRow: number, maxCol: number, totalBirds: number) {
    let data: Array<{ x: number; y: number }> = [];
    for (let i = 0; i < totalBirds; i++) {
      let unique = false;
      while (!unique) {
        let newX = Math.floor(Math.random() * maxRow);
        let newY = Math.floor(Math.random() * maxCol);
        if (!data.some(bird => bird.x == newX && bird.y == newY)) {
          data.push({x: newX, y: newY});
          unique = true;
        }
      }
    }
    return data;
  }

  function FindAdjacentBirds(x: number, y: number) {
    let count = 0;
    
    // Check row (x) above
    if (y > 0) {
      if (birdLocations.some(bird => bird.x == x - 1 && bird.y == y - 1)) count++;
      if (birdLocations.some(bird => bird.x == x     && bird.y == y - 1)) count++;
      if (birdLocations.some(bird => bird.x == x + 1 && bird.y == y - 1)) count++;
    }

    // Check same row (x)
    if (birdLocations.some(bird => bird.x == x - 1 && bird.y == y)) count++;
    if (birdLocations.some(bird => bird.x == x     && bird.y == y)) count++;
    if (birdLocations.some(bird => bird.x == x + 1 && bird.y == y)) count++;

    // Check row (x) below
    if (birdLocations.some(bird => bird.x == x - 1 && bird.y == y + 1)) count++;
    if (birdLocations.some(bird => bird.x == x     && bird.y == y + 1)) count++;
    if (birdLocations.some(bird => bird.x == x + 1 && bird.y == y + 1)) count++;

    return count;
  }

</script>

<div class="main-container">
  <div class="game-container">
    <div class="tbl">
      {#each { length: fieldRows }, row}
        <div class="row">
          {#each { length: fieldCols }, col}
            <div class="cell">
              <button class="cell-button" id="{row}-{col}" onclick={() => ShowCell(col, row)}>
                <!-- {#if !cellClicked}
                  <div class="cell-unclicked">No</div>
                {:else}
                  <div class="cell-clicked">yes</div>
                {/if} -->
                
                {#if birdLocations.some(bird => bird.x == col && bird.y == row)}
                  <Bird />
                {:else}
                  {@const adjacentBirds = FindAdjacentBirds(col, row)}
                  {#if adjacentBirds == 1}
                    <One />
                  {:else if adjacentBirds == 2}
                    <Two />
                  {:else if adjacentBirds == 3}
                    <Three />
                  {:else if adjacentBirds == 4}
                    <Four />
                  {:else if adjacentBirds == 5}
                    <Five />
                  {:else if adjacentBirds == 6}
                    <Six />
                  {:else if adjacentBirds == 7}
                    <Seven />
                  {:else if adjacentBirds == 8}
                    <Eight />
                  {:else}
                    <div class="cell-clicked"></div>
                  {/if}
                {/if}
              </button>
            </div>
          {/each}
        </div>
      {/each}
    </div>
  </div>
</div>


<style>
  .main-container {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
  }

  .game-container {
    display: flex;
    justify-content: center;
    align-items: center;
    border: 1px solid blue;
    width: 800px;
    height: 800px;
  }

  .tbl {
    display: flex;
    flex-direction: column;
  }

  .row {
    display: flex;
    height: 60px;
    justify-content: center;
  }

  .cell {
    display: flex;
    width: 60px;
    border: 1px solid bisque;
    background-color: rgb(123, 163, 239);
    align-items: center;
    justify-content: center;
  }

  .cell-unclicked {
    height: 54px;
    width: 54px;
    background-color: rgb(211, 211, 211);
  }

  .cell-clicked {
    height: 54px;
    width: 54px;
    background-color: rgb(123, 163, 239);
  }

  .cell-button {
    background-color: transparent;
    border: 0;
  }
</style>
