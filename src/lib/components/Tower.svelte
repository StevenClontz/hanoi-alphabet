<script lang="ts">
    import Disc from "./Disc.svelte";
	let { color = "blue" } = $props();
</script>

<div class="w-1/3 h-full flex flex-col-reverse" style="background-color: {color}">
    <div class="flex flex-col p-2">
        <Disc width="70%" letter="A"/>
        <Disc width="80%" letter="B"/>
        <Disc width="90%" letter="C"/>
    </div>
</div>