<script lang="ts">
	import { onMount } from "svelte";
	import Footer from "$lib/components/Footer.svelte";
	import Header from "$lib/components/Header.svelte";
	import Hero from "$lib/components/Hero.svelte";

	let ExamplesGrid: typeof import("$lib/components/home/ExamplesGrid.svelte").default | null =
		$state(null);
	let examplesLoadFailed = $state(false);

	onMount(() => {
		const loadExamples = async () => {
			try {
				const module = await import("$lib/components/home/ExamplesGrid.svelte");
				ExamplesGrid = module.default;
			} catch {
				examplesLoadFailed = true;
			}
		};

		if ("requestIdleCallback" in window) {
			window.requestIdleCallback(() => void loadExamples());
			return;
		}

		setTimeout(() => void loadExamples(), 150);
	});
</script>

<svelte:head>
	<title>mapcn-svelte - Beautiful maps made simple</title>
</svelte:head>

<div class="flex flex-col">
	<Header />

	<main class="flex-1 pb-32">
		<section class="relative w-full py-20">
			<Hero />
		</section>

		<section class="container px-6">
			{#if ExamplesGrid}
				<ExamplesGrid />
			{:else if examplesLoadFailed}
				<p class="text-muted-foreground py-8 text-center text-sm">
					Unable to load interactive examples right now.
				</p>
			{:else}
				<div class="grid grid-cols-1 gap-5 sm:grid-cols-2 lg:grid-cols-4">
					<div
						class="bg-card border-border/50 h-56 animate-pulse rounded-xl border sm:col-span-2"
					></div>
					<div class="bg-card border-border/50 h-56 animate-pulse rounded-xl border"></div>
					<div class="bg-card border-border/50 h-56 animate-pulse rounded-xl border"></div>
					<div class="bg-card border-border/50 h-56 animate-pulse rounded-xl border"></div>
					<div class="bg-card border-border/50 h-56 animate-pulse rounded-xl border"></div>
				</div>
			{/if}
		</section>
	</main>

	<Footer />
</div>
