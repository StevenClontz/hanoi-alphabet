<script lang="ts">
	import './layout.css';
	import favicon from '$lib/assets/favicon.svg';

	let { children } = $props();
</script>

<svelte:head><link rel="icon" href={favicon} /></svelte:head>

<main class="h-screen w-full bg-yellow-100 flex items-center justify-center">
	<div class="size-full sm:h-100 sm:w-2/3 lg:w-1/2 bg-green-100 flex">
		{@render children()}
	</div>
</main>
