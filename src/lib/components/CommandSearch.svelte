<script lang="ts">
	import { Search } from "lucide-svelte";
	import { CommandDialog, CommandEmpty, CommandGroup, CommandInput, CommandItem, CommandList } from "$lib/registry/ui/command/index.js";
	import { Kbd } from "$lib/registry/ui/kbd/index.js";
	import {
		Map,
		BookOpen,
		Code,
		Braces,
		MapPin,
		MessageSquare,
		Route,
		Settings,
	} from "lucide-svelte";

	let open = $state(false);

	const docsNavigation = [
		{
			title: "Basics",
			items: [
				{ title: "Getting Started", href: "/docs", icon: BookOpen },
				{ title: "Installation", href: "/docs/installation", icon: Code },
				{ title: "API Reference", href: "/docs/api-reference", icon: Braces },
			],
		},
		{
			title: "Examples",
			items: [
				{ title: "Basic Map", href: "/docs/basic-map", icon: Map },
				{ title: "Map Controls", href: "/docs/controls", icon: Settings },
				{ title: "Markers", href: "/docs/markers", icon: MapPin },
				{ title: "Popups", href: "/docs/popups", icon: MessageSquare },
				{ title: "Routes", href: "/docs/routes", icon: Route },
			],
		},
	];

	function handleKeyDown(e: KeyboardEvent) {
		if ((e.metaKey || e.ctrlKey) && e.key === "k") {
			e.preventDefault();
			open = !open;
		}
	}

	function handleSelect(href: string) {
		open = false;
		window.location.href = href;
	}

	// Set up keyboard event listener with cleanup
	$effect(() => {
		document.addEventListener("keydown", handleKeyDown);
		return () => document.removeEventListener("keydown", handleKeyDown);
	});
</script>

<button
	onclick={() => (open = true)}
	aria-label="Search documentation"
	class="hidden group sm:flex items-center gap-2 h-8 px-3 rounded-full bg-secondary/60 border border-border/40 text-sm text-muted-foreground hover:text-foreground hover:border-border transition-colors"
>
	<Search class="size-3.5" />
	<span class="hidden sm:inline">Search...</span>
	<Kbd class="hidden sm:inline-flex ml-2">⌘K</Kbd>
</button>

<CommandDialog
	bind:open
	title="Search Documentation"
	description="Search for documentation pages and components"
>
	<CommandInput placeholder="Type to search..." />
	<CommandList>
		<CommandEmpty>No results found.</CommandEmpty>
		{#each docsNavigation as group}
			<CommandGroup heading={group.title}>
				{#each group.items as item}
					<CommandItem
						value={item.title}
						onselect={() => handleSelect(item.href)}
					>
						<item.icon class="size-4" />
						<span>{item.title}</span>
					</CommandItem>
				{/each}
			</CommandGroup>
		{/each}
	</CommandList>
</CommandDialog>
