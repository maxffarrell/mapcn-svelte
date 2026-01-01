<script lang="ts">
	import { Button } from "$lib/registry/ui/button/index";

	const { command }: { command: string } = $props();

	let copied = $state(false);

	const copy = () => {
		navigator.clipboard.writeText(command);
		copied = true;
		setTimeout(() => copied = false, 2000);
	};
</script>

<Button onclick={copy}>
	{#if copied}
		Copied
	{:else}
		Copy
	{/if}
</Button>
