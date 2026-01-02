<script lang="ts">
	import { cn } from "$lib/utils";

	interface Props {
		class?: string;
	}

	const { class: className }: Props = $props();
</script>

<footer class={cn("w-full border-t border-border/40 px-6 py-6", className)}>
	<div
		class="mx-auto flex max-w-5xl items-center justify-between text-xs text-muted-foreground"
	>
		<span>
			Built by
			<a
				href="https://github.com/AnmolSaini16"
				target="_blank"
				rel="noopener noreferrer"
				class="font-medium text-foreground hover:underline"
			>
				Anmol
			</a>.
			Ported to Svelte by
			<a
				href="https://github.com/MariusLang"
				target="_blank"
				rel="noopener noreferrer"
				class="font-medium text-foreground hover:underline"
			>
				Marius Lang
			</a>.
		</span>
		<div class="flex items-center gap-4">
			<a
				href="https://github.com/MariusLang/mapcn-svelte"
				target="_blank"
				rel="noopener noreferrer"
				class="transition-colors hover:text-foreground"
			>
				GitHub
			</a>
			<a href="/docs" class="transition-colors hover:text-foreground">
				Docs
			</a>
		</div>
	</div>
</footer>
