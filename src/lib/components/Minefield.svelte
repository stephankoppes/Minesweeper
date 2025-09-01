<script lang="ts">
    let fieldCols = 8;
    let fieldRows = 8;
</script>

<h3>Minefield</h3>

<div class="tbl">
    {#each { length: fieldRows }, row}
        <div class="row">
            {#each { length: fieldCols }, col}
                <div class="cell">{row}-{col}</div>
            {/each}
        </div>
    {/each}
</div>

<style>
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
    border: 1px solid blue;
    background-color: cornflowerblue;
    color: white;
    align-items: center;
    justify-content: center;
}
</style>