<script lang="ts">
	import Droppable from '$lib/components/droppable.svelte';
	import SortableItem from './sortable-item.svelte';
	import {CollisionPriority} from '@dnd-kit/abstract';
	import {DragDropProvider, DragOverlay} from '@dnd-kit-svelte/svelte';
	import {move} from '@dnd-kit/helpers';
	import {sensors} from '$lib';
	import {RestrictToWindowEdges} from '@dnd-kit-svelte/svelte/modifiers';

	interface Todo {
		id: string;
		content: string;
		size: number;
	}

	const allItems = [
			{id: 'A', content: 'A', size: 1},
			{id: 'B', content: 'B', size: 2},
			{id: 'C', content: 'C', size: 3},
			{id: 'D', content: 'D', size: 4},
	]

	const items = {
		left: allItems,
		middle: [],
		right: [],
	};

	const taskWidth = (todo:Todo) => {
		const maxSize = Math.max(...allItems.map(i=>i.size));
		return 95 + (todo.size - maxSize)*10
	}

	type Todos = Record<string, Todo[]>;
	let todos = $state<Todos>(items);
</script>

<DragDropProvider
	{sensors}
	modifiers={[RestrictToWindowEdges]}
	onDragOver={(event) => {
		todos = move(todos, event);
	}}
>
	<div class="grid gap-4 grid-cols-3 h-full">
		{@render taskList('left', todos['left'])}
		{@render taskList('middle', todos['middle'])}
		{@render taskList('right', todos['right'])}
	</div>

	<DragOverlay>
		{#snippet children(source)}
			{@const task = todos[source.data.group].find((todo) => todo.id === source.id)!}
			<SortableItem id={task.id} 
				{task}
				width={taskWidth(task)} 
				index={0} 
				isOverlay />
		{/snippet}
	</DragOverlay>
</DragDropProvider>

{#snippet taskList(id: string, tasks: Todo[])}
	<Droppable
		class="bg-gray-50 border border-gray-200 rd-3xl p-3 pt-6 h-full"
		{id}
		type="column"
		accept="item"
		collisionPriority={CollisionPriority.Low}
	>
		<div class="h-full flex flex-col-reverse">
			<div class="grid gap-2">
				{#each tasks as task, index (task.id)}
					<SortableItem {task} 
						width={taskWidth(task)} 
						id={task.id} 
						index={() => index} 
						group={id} 
						data={{group: id}} 
						type="item" />
				{/each}
			</div>
		</div>
	</Droppable>
{/snippet}
