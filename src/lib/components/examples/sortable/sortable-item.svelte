<script lang="ts">
	import {useSortable, type UseSortableInput} from '@dnd-kit-svelte/svelte/sortable';

	interface Task {
		id: string | number;
		content: string;
		size: number;
	}

	interface Props extends UseSortableInput {
		task: Task;
		width: number;
		isOverlay?: boolean;
		editable?: boolean;
	}

	let {task, width, isOverlay = false, editable = false, ...rest}: Props = $props();

	const {ref, isDragging} = useSortable({...rest, feedback: 'move'});
</script>

<div class="relative select-none h-12" {@attach ref}>
	<!-- Original element - becomes invisible during drag but maintains dimensions -->
	<div class={['p-4 bg-white rd-18px text-center mx-auto', {invisible: isDragging.current && !isOverlay}]}
		style="width: {width}%">
		{#if editable}
			<input type="text" 
				bind:value={task.content} 
				class="w-5 text-center border-dotted border-b-1 border-gray-400"
				maxlength="1" />
		{:else}
			{task.content}
		{/if}
	</div>

	<!-- Drag placeholder - set to match original dimensions -->
	{#if !isOverlay && isDragging.current}
		<div class="flex items-center justify-center abs inset-0">
			<!-- You can put any content here for the dragging state -->
			<div class="w-full h-full p-2 bg-sky/10 rd-18px b-2 b-sky/5 flex items-center justify-center mx-auto"
				style="width: {width}%">
				<span class="text-sky">{task.content}</span>
			</div>
		</div>
	{/if}
</div>
