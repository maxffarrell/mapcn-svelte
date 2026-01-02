<script lang="ts">
	import ComponentPreviewClient from "$lib/components/docs/preview/ComponentPreviewClient.svelte";

	const {
		code,
		highlightedCode,
		class: className,
		children,
	} = $props<{
		code: string;
		highlightedCode: string;
		class?: string;
		children?: import("svelte").Snippet;
	}>();
</script>

<ComponentPreviewClient
	{code}
	{highlightedCode}
	{className}
>
	{@render children?.()}
</ComponentPreviewClient>
