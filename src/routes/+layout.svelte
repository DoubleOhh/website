<script lang="ts">
	import '../app.css';

	import favicon from '$lib/assets/favicon.svg';
	import Header from '$lib/components/Header.svelte';

	let { children } = $props();
	let y = $state(0);
</script>

<svelte:head><title>One of One</title><link rel="icon" href={favicon} /></svelte:head>

<svelte:window bind:scrollY={y} />

<Header {y} />
{@render children()}
