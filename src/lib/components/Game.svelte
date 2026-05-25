<script lang="ts">
	interface Disc {
		id: string;
		content: string;
		size: number;
	}

	const allDiscs:Disc[] = [
			{id: 'A', content: 'A', size: 1},
			{id: 'B', content: 'A', size: 2},
			{id: 'C', content: 'A', size: 3},
			{id: 'D', content: 'A', size: 4},
	]

	const initialTowers = {
		left: allDiscs,
		middle: [],
		right: [],
	};

	const discWidth = (disc:Disc) => {
		const maxSize = Math.max(...allDiscs.map(i=>i.size));
		return 95 + (disc.size - maxSize)*10
	}

	type DiscRecord = Record<string, Disc[]>;
	let towerState = $state<DiscRecord>(initialTowers);
	let start = $state<Boolean>(false);

	const startGame = () => {
		start = true
		towerState.left = towerState.left.map((t) => {
			t.content = t.content.toUpperCase()
			if (!/^[A-Z]$/.test(t.content)) {
				t.content = "A"
			}
			return t
		})
	}

	const resetGame = () => {
		start = false
		towerState = initialTowers
	}

	const canMoveFrom = (disc:Disc,towerId:string) => {
		return (towerState[towerId].includes(disc)) && (disc.size === Math.min(...towerState[towerId].map(d=>d.size)))
	}

	const canMoveTo = (disc:Disc,towerId:string) => {
		return disc.size < Math.min(...towerState[towerId].map(d=>d.size))
	}

	const moveFromTo = (disc:Disc,fromId:string,toId:string,undo=false) => {
		if (
			canMoveFrom(disc,fromId) &&
			canMoveTo(disc,toId)
		) {
			cycleContent(disc)
			towerState[fromId] = towerState[fromId].filter(d=>d!==disc)
			towerState[toId] = [disc,...towerState[toId]]
		}
	}

	const moveRight = (disc:Disc) => {
		moveFromTo(disc,"middle","right")
		moveFromTo(disc,"left","middle")
	}

	const moveFarRight = (disc:Disc) => {
		moveFromTo(disc,"left","right")
	}

	const moveLeft = (disc:Disc) => {
		moveFromTo(disc,"middle","left")
		moveFromTo(disc,"right","middle")
	}

	const moveFarLeft = (disc:Disc) => {
		moveFromTo(disc,"right","left")
	}

	const cycleContent = (disc:Disc) => {
		disc.content = String.fromCharCode((disc.content.charCodeAt(0)-"A".charCodeAt(0)+1)%26 +"A".charCodeAt(0))
	}
</script>

<div class="h-full">
	<div class="grid gap-4 grid-cols-3 justify-center items-center h-9/10">
		{@render towerColumn('left', towerState.left)}
		{@render towerColumn('middle', towerState.middle)}
		{@render towerColumn('right', towerState.right)}
	</div>
	<div class="h-1/10 flex items-center justify-center">
		{#if start}
			<button onclick={resetGame} 
				class="w-20 h-full p-3 mx-auto text-center border border-gray-100 bg-gray-50 rounded-lg">
				Reset</button>
		{:else}
			<button onclick={startGame} 
				class="w-20 h-full p-3 mx-auto text-center border border-gray-200 bg-gray-100 rounded-lg">
				Start</button>
		{/if}
	</div>
</div>

{#snippet towerColumn(id: string, discs: Disc[])}
	<div class="h-4/5 flex flex-col-reverse bg-gray-200 border border-gray-400 rd-3xl px-3 py-6 select-none">
		<div class="grid gap-4">
			{#each discs as disc}
				<div class="flex-col p-2 bg-gray-50 rd-18px text-center mx-auto"
					style="width: {discWidth(disc)}%">
					<div class="w-full flex items-center justify-between">
						<span>
							{#if start}
								<!-- <button class="p-1 border rounded-sm border-gray-200"
									onclick={()=>moveFarLeft(disc)}>&laquo;</button> -->
								<button class="p-1 w-5 md:w-10 text-center border rounded-sm border-gray-200"
									onclick={()=>moveLeft(disc)}>&lt;</button>
							{/if}
						</span>
						{#if !start}
							<input type="text" 
								bind:value={disc.content} 
								class="w-5 text-center border-dotted border-b-1 border-gray-400"
								maxlength="1" />
						{:else}
							<span>{disc.content}</span>
						{/if}
						<span>
							{#if start}
								<button class="p-1 w-5 md:w-10 text-center border rounded-sm border-gray-200"
									onclick={()=>moveRight(disc)}>&gt;</button>
								<!-- <button class="p-1 border rounded-sm border-gray-200"
									onclick={()=>moveFarRight(disc)}>&raquo;</button> -->
							{/if}
						</span>
					</div>
					<div class="text-xs">{disc.size}</div>
				</div>
			{/each}
		</div>
	</div>
{/snippet}
