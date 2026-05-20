<script lang="ts">
	let { width = "90%", letter="X" } = $props();
</script>

<div class="text-center rounded-md my-2 mx-auto border bg-gray-100 cursor-pointer select-none" style="width: {width}">
    {letter}
</div>
