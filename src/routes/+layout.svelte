<script lang="ts">
	import '@unocss/reset/tailwind.css';
	import 'uno.css';
	import '../app.css';

	let {children} = $props();
</script>

<main class="px-5 py-10 h-screen w-full">
	<div class="flex justify-center items-center size-full">
		{@render children()}
	</div>
</main>
