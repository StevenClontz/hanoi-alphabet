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

	let start = $state<Boolean>(false);

	const startGame = () => {
		start = true
		todos.left = todos.left.map((t) => {
			t.content = t.content.toUpperCase()
			if (!/^[A-Z]$/.test(t.content)) {
				t.content = "A"
			}
			return t
		})
	}

	const disabledTask = (task:Todo,tasks:Todo[]) => {
		return !start || task.size !== Math.min(...tasks.map(t=>t.size))
	}
</script>

<DragDropProvider
	{sensors}
	modifiers={[RestrictToWindowEdges]}
	onDragOver={(event) => {
		todos = move(todos, event);
	}}
>
	<div class="grid gap-4 grid-cols-3 h-full justify-center items-center">
		{#if start}
			{@render taskList('left', todos['left'])}
			{@render taskList('middle', todos['middle'])}
			{@render taskList('right', todos['right'])}
		{:else}
			{@render taskColumn('left', todos['left'])}
			{@render taskColumn('middle', todos['middle'])}
			{@render taskColumn('right', todos['right'])}
			<div></div>
			<button onclick={startGame} 
				class="w-20 h-10 p-3 mx-auto text-center border border-gray-100 bg-gray-50 rounded-lg">
				Start</button>
		{/if}
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
		class="h-full"
		{id}
		type="column"
		accept="item"
		collisionPriority={CollisionPriority.Low}
	>
		{@render taskColumn(id,tasks)}
	</Droppable>
{/snippet}

{#snippet taskColumn(id: string, tasks: Todo[])}
	<div class="h-full flex flex-col-reverse bg-gray-50 border border-gray-200 h-full rd-3xl p-3 pt-6">
		<div class="grid gap-2">
			{#each tasks as task, index (task.id)}
				<SortableItem {task} 
					width={taskWidth(task)} 
					id={task.id} 
					index={() => index} 
					group={id}
					editable={!start}
					disabled={{draggable: disabledTask(task,tasks)}}
					data={{group: id}} 
					type="item" />
			{/each}
		</div>
	</div>
{/snippet}
