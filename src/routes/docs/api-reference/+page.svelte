<script lang="ts">
	import DocsLayout from "$lib/components/docs/DocsLayout.svelte";
	import DocsNote from "$lib/components/docs/DocsNote.svelte";
	import DocsLink from "$lib/components/docs/DocsLink.svelte";
	import DocsSection from "$lib/components/docs/DocsSection.svelte";
	import { CodeBlock } from "$lib/components/docs/preview";
	import DocsCode from "$lib/components/docs/DocsCode.svelte";
	import DocsPropTable from "$lib/components/docs/DocsPropTable.svelte";

	const anatomyCode = `<Map>
  <MapClusterLayer data={...} />

  <MapMarker longitude={...} latitude={...}>
    <MarkerContent>
      <MarkerLabel />
    </MarkerContent>
    <MarkerPopup />
    <MarkerTooltip />
  </MapMarker>

  <MapPopup longitude={...} latitude={...} />
  <MapControls />
  <MapRoute coordinates={...} />
</Map>`;

	const useMapCode =
		`<scr` +
		`ipt lang="ts">
  import { getContext } from "svelte";
  import type MapLibreGL from "maplibre-gl";

  const mapCtx = getContext<{
    getMap: () => MapLibreGL.Map | null;
    isLoaded: () => boolean;
  }>("map");

  // Access the map instance
  const map = mapCtx.getMap();
  const isLoaded = mapCtx.isLoaded();
</scr` +
		`ipt>`;
</script>

<svelte:head>
	<title>API Reference - mapcn-svelte</title>
</svelte:head>

<DocsLayout
	title="API Reference"
	description="Complete reference for all map components and their props."
	prev={{ title: "Installation", href: "/docs/installation" }}
	next={{ title: "Map", href: "/docs/basic-map" }}
>
	<DocsNote>
		<strong>Note:</strong> This library is built on top of
		<DocsLink href="https://maplibre.org/maplibre-gl-js/docs/API/" external>
			MapLibre GL JS
		</DocsLink>
		. Most components extend the native MapLibre options. Refer to the
		<DocsLink href="https://maplibre.org/maplibre-gl-js/docs/API/classes/Map/" external>
			MapLibre Map API
		</DocsLink>
		for additional options not listed here.
	</DocsNote>

	<DocsSection title="Component Anatomy">
		<p>All parts of the component that you can use and combine to build your map.</p>

		<CodeBlock code={anatomyCode} showCopyButton={false} />
	</DocsSection>

	<!-- Map -->
	<DocsSection title="Map">
		<p>
			The root container component that initializes MapLibre GL and provides context to child
			components. Automatically handles theme switching between light and dark modes.
		</p>

		<p>
			Extends
			<DocsLink
				href="https://maplibre.org/maplibre-gl-js/docs/API/type-aliases/MapOptions/"
				external
			>
				MapOptions
			</DocsLink>
			from MapLibre GL (excluding
			<DocsCode>container</DocsCode>
			and
			<DocsCode>style</DocsCode>
			). Additional options can be passed via the
			<DocsCode>options</DocsCode>
			prop.
		</p>

		<DocsPropTable
			props={[
				{
					name: "children",
					type: "Snippet",
					description: "Child components (markers, popups, controls, routes).",
				},
				{
					name: "theme",
					type: '"light" | "dark"',
					description: "Theme for the map. If not provided, automatically detects system preference.",
				},
				{
					name: "styles",
					type: "{ light?: string | StyleSpecification; dark?: string | StyleSpecification }",
					description: "Custom map styles for light and dark themes.",
				},
				{
					name: "projection",
					type: "ProjectionSpecification",
					description: "Map projection type. Use { type: 'globe' } for 3D globe view.",
				},
				{
					name: "center",
					type: "[number, number]",
					default: "[13.405, 52.52]",
					description: "Initial map center [longitude, latitude].",
				},
				{
					name: "zoom",
					type: "number",
					default: "0",
					description: "Initial map zoom level.",
				},
				{
					name: "options",
					type: "Omit<MapOptions, 'container' | 'style'>",
					description: "Additional MapLibre MapOptions (excluding container and style).",
				},
			]}
		/>
	</DocsSection>

	<!-- MapClusterLayer -->
	<DocsSection title="MapClusterLayer">
		<p>
			Renders clustered GeoJSON points on the map. Must be used inside
			<DocsCode>Map</DocsCode>
			.
		</p>

		<DocsPropTable
			props={[
				{
					name: "data",
					type: "string | GeoJSON.FeatureCollection<Point, P>",
					description: "GeoJSON FeatureCollection data or URL to fetch GeoJSON from.",
				},
				{
					name: "clusterMaxZoom",
					type: "number",
					default: "14",
					description: "Maximum zoom level to cluster points on.",
				},
				{
					name: "clusterRadius",
					type: "number",
					default: "50",
					description: "Radius of each cluster when clustering points in pixels.",
				},
				{
					name: "clusterColors",
					type: "[string, string, string]",
					default: '["#51bbd6", "#f1f075", "#f28cb1"]',
					description: "Colors for cluster circles: [small, medium, large] based on point count.",
				},
				{
					name: "clusterThresholds",
					type: "[number, number]",
					default: "[100, 750]",
					description: "Point count thresholds for color/size steps: [medium, large].",
				},
				{
					name: "pointColor",
					type: "string",
					default: '"#3b82f6"',
					description: "Color for unclustered individual points.",
				},
				{
					name: "onpointclick",
					type: "(feature: Feature<Point, P>, coordinates: [number, number]) => void",
					description: "Callback when an unclustered point is clicked.",
				},
				{
					name: "onclusterclick",
					type: "(clusterId: number, coordinates: [number, number], pointCount: number) => void",
					description: "Callback when a cluster is clicked. If not provided, zooms into the cluster.",
				},
			]}
		/>
	</DocsSection>

	<!-- MapMarker -->
	<DocsSection title="MapMarker">
		<p>
			Renders a draggable marker at a specific coordinate. Must be used inside
			<DocsCode>Map</DocsCode>
			.
		</p>

		<DocsPropTable
			props={[
				{
					name: "longitude",
					type: "number",
					description: "Marker longitude coordinate.",
				},
				{
					name: "latitude",
					type: "number",
					description: "Marker latitude coordinate.",
				},
				{
					name: "children",
					type: "Snippet",
					description: "Child components (MarkerContent, MarkerLabel, MarkerPopup, MarkerTooltip).",
				},
				{
					name: "onclick",
					type: "(e: MouseEvent) => void",
					description: "Callback when marker is clicked.",
				},
				{
					name: "onmouseenter",
					type: "(e: MouseEvent) => void",
					description: "Callback when mouse enters marker.",
				},
				{
					name: "onmouseleave",
					type: "(e: MouseEvent) => void",
					description: "Callback when mouse leaves marker.",
				},
				{
					name: "ondragstart",
					type: "(lngLat: { lng: number; lat: number }) => void",
					description: "Callback when drag starts.",
				},
				{
					name: "ondrag",
					type: "(lngLat: { lng: number; lat: number }) => void",
					description: "Callback while dragging.",
				},
				{
					name: "ondragend",
					type: "(lngLat: { lng: number; lat: number }) => void",
					description: "Callback when drag ends.",
				},
				{
					name: "draggable",
					type: "boolean",
					default: "false",
					description: "Whether the marker is draggable.",
				},
				{
					name: "anchor",
					type: '"center" | "top" | "bottom" | "left" | "right" | "top-left" | "top-right" | "bottom-left" | "bottom-right"',
					default: '"bottom"',
					description: "Point on the marker that anchors to the coordinate.",
				},
				{
					name: "offset",
					type: "PointLike",
					description: "Offset from the anchor point.",
				},
				{
					name: "rotation",
					type: "number",
					description: "Marker rotation in degrees.",
				},
				{
					name: "pitchAlignment",
					type: '"map" | "viewport" | "auto"',
					description: "Pitch alignment behavior.",
				},
				{
					name: "rotationAlignment",
					type: '"map" | "viewport" | "auto"',
					description: "Rotation alignment behavior.",
				},
			]}
		/>
	</DocsSection>

	<!-- MarkerContent -->
	<DocsSection title="MarkerContent">
		<p>
			Custom content for a
			<DocsCode>MapMarker</DocsCode>
			. Must be used inside
			<DocsCode>MapMarker</DocsCode>
			.
		</p>

		<DocsPropTable
			props={[
				{
					name: "children",
					type: "Snippet",
					description: "Custom marker content.",
				},
				{
					name: "className",
					type: "string",
					description: "Additional CSS classes.",
				},
			]}
		/>
	</DocsSection>

	<!-- MarkerLabel -->
	<DocsSection title="MarkerLabel">
		<p>
			Text label for a
			<DocsCode>MapMarker</DocsCode>
			. Must be used inside
			<DocsCode>MapMarker</DocsCode>
			.
		</p>

		<DocsPropTable
			props={[
				{
					name: "children",
					type: "Snippet",
					description: "Label text content.",
				},
				{
					name: "className",
					type: "string",
					description: "Additional CSS classes.",
				},
				{
					name: "position",
					type: '"top" | "bottom"',
					default: '"top"',
					description: "Position of the label relative to the marker.",
				},
			]}
		/>
	</DocsSection>

	<!-- MarkerPopup -->
	<DocsSection title="MarkerPopup">
		<p>
			Popup attached to a
			<DocsCode>MapMarker</DocsCode>
			. Must be used inside
			<DocsCode>MapMarker</DocsCode>
			.
		</p>

		<DocsPropTable
			props={[
				{
					name: "children",
					type: "Snippet",
					description: "Popup content.",
				},
				{
					name: "className",
					type: "string",
					description: "Additional CSS classes.",
				},
				{
					name: "closeButton",
					type: "boolean",
					default: "false",
					description: "Show close button.",
				},
				{
					name: "offset",
					type: "PointLike",
					default: "16",
					description: "Offset from the anchor point.",
				},
				{
					name: "anchor",
					type: '"top" | "bottom" | "left" | "right" | "top-left" | "top-right" | "bottom-left" | "bottom-right"',
					description: "Popup anchor position.",
				},
				{
					name: "closeOnClick",
					type: "boolean",
					description: "Close popup when map is clicked.",
				},
				{
					name: "closeOnMove",
					type: "boolean",
					description: "Close popup when map is moved.",
				},
				{
					name: "focusAfterOpen",
					type: "boolean",
					description: "Focus popup after opening.",
				},
				{
					name: "maxWidth",
					type: "string",
					description: "Maximum width of the popup.",
				},
			]}
		/>
	</DocsSection>

	<!-- MarkerTooltip -->
	<DocsSection title="MarkerTooltip">
		<p>
			Tooltip that appears on hover over a
			<DocsCode>MapMarker</DocsCode>
			. Must be used inside
			<DocsCode>MapMarker</DocsCode>
			.
		</p>

		<DocsPropTable
			props={[
				{
					name: "children",
					type: "Snippet",
					description: "Tooltip content.",
				},
				{
					name: "className",
					type: "string",
					description: "Additional CSS classes.",
				},
				{
					name: "offset",
					type: "PointLike",
					default: "16",
					description: "Offset from the anchor point.",
				},
				{
					name: "anchor",
					type: '"top" | "bottom" | "left" | "right" | "top-left" | "top-right" | "bottom-left" | "bottom-right"',
					description: "Tooltip anchor position.",
				},
			]}
		/>
	</DocsSection>

	<!-- MapPopup -->
	<DocsSection title="MapPopup">
		<p>
			Standalone popup at specific coordinates (not attached to a marker). Must be used inside
			<DocsCode>Map</DocsCode>
			.
		</p>

		<DocsPropTable
			props={[
				{
					name: "longitude",
					type: "number",
					description: "Popup longitude coordinate.",
				},
				{
					name: "latitude",
					type: "number",
					description: "Popup latitude coordinate.",
				},
				{
					name: "children",
					type: "Snippet",
					description: "Popup content.",
				},
				{
					name: "className",
					type: "string",
					description: "Additional CSS classes.",
				},
				{
					name: "closeButton",
					type: "boolean",
					default: "false",
					description: "Show close button.",
				},
				{
					name: "onclose",
					type: "() => void",
					description: "Callback when popup is closed.",
				},
				{
					name: "offset",
					type: "PointLike",
					default: "12",
					description: "Offset from the anchor point.",
				},
				{
					name: "anchor",
					type: '"top" | "bottom" | "left" | "right" | "top-left" | "top-right" | "bottom-left" | "bottom-right"',
					description: "Popup anchor position.",
				},
				{
					name: "closeOnClick",
					type: "boolean",
					description: "Close popup when map is clicked.",
				},
				{
					name: "closeOnMove",
					type: "boolean",
					description: "Close popup when map is moved.",
				},
				{
					name: "focusAfterOpen",
					type: "boolean",
					description: "Focus popup after opening.",
				},
				{
					name: "maxWidth",
					type: "string",
					description: "Maximum width of the popup.",
				},
			]}
		/>
	</DocsSection>

	<!-- Map Context -->
	<DocsSection title="Map Context">
		<p>
			Access the MapLibre map instance and loading state via Svelte context. Must be used within a
			<DocsCode>Map</DocsCode>
			component.
		</p>

		<CodeBlock code={useMapCode} language="svelte" showCopyButton={false} />

		<p>
			Provides
			<DocsCode>getMap()</DocsCode>
			returning the
			<DocsLink href="https://maplibre.org/maplibre-gl-js/docs/API/classes/Map/" external>
				MapLibre.Map
			</DocsLink>
			instance, and
			<DocsCode>isLoaded()</DocsCode>
			returning a boolean.
		</p>
	</DocsSection>

	<!-- MapControls -->
	<DocsSection title="MapControls">
		<p>
			Renders map control buttons (zoom, compass, locate, fullscreen). Must be used inside
			<DocsCode>Map</DocsCode>
			.
		</p>

		<DocsPropTable
			props={[
				{
					name: "position",
					type: '"top-left" | "top-right" | "bottom-left" | "bottom-right"',
					default: '"bottom-right"',
					description: "Position of the controls.",
				},
				{ name: "showZoom", type: "boolean", default: "true", description: "Show zoom buttons." },
				{
					name: "showCompass",
					type: "boolean",
					default: "false",
					description: "Show compass button.",
				},
				{
					name: "showLocate",
					type: "boolean",
					default: "false",
					description: "Show locate button.",
				},
				{
					name: "showFullscreen",
					type: "boolean",
					default: "false",
					description: "Show fullscreen button.",
				},
				{ name: "className", type: "string", description: "Additional CSS classes." },
				{
					name: "onLocate",
					type: "(coords: { longitude: number; latitude: number }) => void",
					description: "Called when user is located.",
				},
			]}
		/>
	</DocsSection>

	<!-- MapRoute -->
	<DocsSection title="MapRoute">
		<p>
			Renders a line on the map connecting coordinate points. Must be used inside
			<DocsCode>Map</DocsCode>
			.
		</p>

		<DocsPropTable
			props={[
				{
					name: "id",
					type: "string",
					description: "Optional unique identifier for the route layer.",
				},
				{
					name: "coordinates",
					type: "[number, number][]",
					description: "Array of [lng, lat] pairs.",
				},
				{ name: "color", type: "string", default: '"#4285F4"', description: "Line color." },
				{ name: "width", type: "number", default: "3", description: "Line width." },
				{ name: "opacity", type: "number", default: "0.8", description: "Line opacity." },
				{
					name: "dashArray",
					type: "[number, number]",
					description: "Dash pattern for dashed lines.",
				},
				{
					name: "onclick",
					type: "() => void",
					description: "Callback when the route line is clicked.",
				},
				{
					name: "onmouseenter",
					type: "() => void",
					description: "Callback when mouse enters the route line.",
				},
				{
					name: "onmouseleave",
					type: "() => void",
					description: "Callback when mouse leaves the route line.",
				},
				{
					name: "interactive",
					type: "boolean",
					default: "true",
					description: "Whether the route is interactive - shows pointer cursor on hover.",
				},
			]}
		/>
	</DocsSection>
</DocsLayout>
