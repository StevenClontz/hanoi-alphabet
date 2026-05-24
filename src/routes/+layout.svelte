<script lang="ts">
	import '@unocss/reset/tailwind.css';
	import 'uno.css';
	import '../app.css';

	let {children} = $props();
</script>

<main class="px-5 py-10 space-y-16">
	{@render children()}
</main>
