<script lang="ts">
	import type {Snippet} from 'svelte';

	interface SectionProps {
		id: string;
		title: string;
		children: Snippet;
	}

	let {id, title, children}: SectionProps = $props();
</script>

<section {id} class="container-base flex|col gap-5">
	<div class="droppable-container">
		{@render children()}
	</div>
</section>
