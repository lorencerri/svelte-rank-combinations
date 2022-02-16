<script>
	import { nanoid } from 'nanoid';
	import { writable } from 'svelte/store';
	import { browser } from '$app/env';

	let state = true;
	const options = writable({});

	const addOption = () => {
		options.update((prev) => ({ ...prev, [nanoid(7)]: '' }));
	};

	if (browser) {
		const fetched = JSON.parse(localStorage.getItem('options'));
		if (fetched) options.set(fetched);

		options.subscribe((data) => {
			localStorage.setItem('options', JSON.stringify(data));
		});
	}
</script>

<div class="hero min-h-screen bg-base-200">
	<div class="artboard phone-2">
		<div class="navbar header bg-base-100">
			<div class="navbar-start">
				<span class="btn btn-ghost normal-case text-xl">
					{#if state}Rankings{:else}Settings{/if}
				</span>
			</div>
			<div class="navbar-end">
				<label class="swap swap-rotate text-4xl">
					<input type="checkbox" on:change={() => (state = !state)} />
					<div class="swap-on">📜</div>
					<div class="swap-off">⚙️</div>
				</label>
			</div>
		</div>

		<div class="text-center pt-1">
			{#if state}
				<div class="grid grid-cols-1 pl-8 pr-8">
					{#each Object.entries($options) as entry}
						<div class="navbar item shadow-lg mb-1 pl-4 pr-4">
							<div class="navbar-start">
								<span>{entry[1]}</span>
							</div>
							<div class="navbar-end">
								<button class="btn btn-sm btn-ghost p-0">
									<svg
										xmlns="http://www.w3.org/2000/svg"
										class="h-6 w-6"
										fill="none"
										viewBox="0 0 24 24"
										stroke="currentColor"
									>
										<path
											stroke-linecap="round"
											stroke-linejoin="round"
											stroke-width="2"
											d="M5 15l7-7 7 7"
										/>
									</svg>
								</button>
								<button class="btn btn-sm btn-primary p-0 ml-2">
									<svg
										xmlns="http://www.w3.org/2000/svg"
										class="h-6 w-6"
										fill="none"
										viewBox="0 0 24 24"
										stroke="currentColor"
									>
										<path
											stroke-linecap="round"
											stroke-linejoin="round"
											stroke-width="2"
											d="M19 9l-7 7-7-7"
										/>
									</svg>
								</button>
							</div>
						</div>
					{/each}
				</div>
			{:else}
				<div class="grid grid-cols-6 pl-8 pr-8">
					{#each Object.entries($options) as entry}
						<div class="col-span-5 pr-2 pb-2">
							<input
								type="text"
								value={entry[1]}
								placeholder="Option Name"
								class="input input-bordered w-full"
								on:input={({ target }) => {
									options.update((prev) => ({ ...prev, [entry[0]]: target.value }));
								}}
							/>
						</div>
						<div>
							<button
								class="btn btn-error w-full"
								on:click={() => {
									options.update((prev) => {
										delete prev[entry[0]];
										return prev;
									});
								}}
							>
								<svg
									xmlns="http://www.w3.org/2000/svg"
									class="h-6 w-6"
									fill="none"
									viewBox="0 0 24 24"
									stroke="currentColor"
								>
									<path
										stroke-linecap="round"
										stroke-linejoin="round"
										stroke-width="2"
										d="M6 18L18 6M6 6l12 12"
									/>
								</svg>
							</button>
						</div>
					{/each}
					<div class="col-span-6">
						<button class="btn btn-success w-full max-w-xs" on:click={addOption}>
							+ New Option
						</button>
					</div>
				</div>
			{/if}
		</div>
	</div>
</div>
