<script lang="ts">
	import "../app.css";
	import Footer from "$lib/components/Footer.svelte";
	import { injectAnalytics } from "@vercel/analytics/sveltekit";

	injectAnalytics({ mode: "production" });

	let { children } = $props();
</script>

{@render children()}
<Footer />
