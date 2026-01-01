<script lang="ts">
	import Header from "$lib/components/Header.svelte";
	import Examples from "$lib/components/Examples.svelte";
	import Hero from "$lib/components/Hero.svelte";
</script>

<div class="mx-auto flex min-h-svh max-w-5xl flex-col gap-8 px-4 py-8">
	<Header className="max-w-5xl mx-auto w-full" />

	<main class="flex-1 px-6 py-16 mx-auto w-full">
		<Hero />
		<Examples />
	</main>
</div>
