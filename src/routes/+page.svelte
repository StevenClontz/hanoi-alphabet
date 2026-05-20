<script lang="ts">
    let discs = [
        {letter: "A", width: "70%"},
        {letter: "B", width: "80%"},
        {letter: "C", width: "90%"},
    ]
    let towers = [
        {discs: discs, color: "blue"},
        {discs: [], color: "red"},
        {discs: [], color: "green"},
    ]
</script>

{#snippet Disc(disc)}
<div class="text-center rounded-md my-2 mx-auto border bg-gray-100 cursor-pointer select-none p-1" style="width: {disc.width}">
    {disc.letter}asdsfd
</div>
{/snippet}

{#snippet Tower(tower)}
<div class="w-1/3 h-full flex flex-col-reverse" style="background-color: {tower.color}">
    <div class="flex flex-col p-2">
        {#each tower.discs as disc}
            {@render Disc(disc)}
        {/each}
    </div>
</div>
{/snippet}

{#each towers as tower}
    {@render Tower(tower)}
{/each}

