<script lang="ts">
    interface DiscI {
        letter: string;
        size: number;
    }
    const discs:DiscI[] = [
        {letter: "A", size: 1},
        {letter: "B", size: 2},
        {letter: "C", size: 3},
    ]
    const discWidth = (disc:DiscI) => {
        return 10*disc.size + (95 - 10*Math.max(...discs.map(d=>d.size)))
    }
    interface TowerI {
        discs: DiscI[];
        color: string;
    }
    const towers:TowerI[] = [
        {discs: Array.from(discs), color: "blue"},
        {discs: [], color: "red"},
        {discs: [], color: "green"},
    ]
</script>

{#snippet Disc(disc:DiscI)}
<div class="text-center rounded-md my-2 mx-auto border bg-gray-100 cursor-pointer select-none p-1" style="width: {discWidth(disc)}%">
    <input type="text" bind:value={disc.letter} maxlength="1" class="inline-block w-6 mx-auto text-center"/>
</div>
{/snippet}

{#snippet Tower(tower:TowerI)}
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

