<script lang="ts">
	import { onMount } from 'svelte';

	type Props = {
		text: string;
		prefix?: string;
		speed?: number;
		startDelay?: number;
	};

	let { text, prefix = '$', speed = 55, startDelay = 350 }: Props = $props();
	let typed = $state('');

	onMount(() => {
		let index = 0;
		let interval: ReturnType<typeof setInterval>;

		const timeout = setTimeout(() => {
			interval = setInterval(() => {
				typed = text.slice(0, index + 1);
				index += 1;

				if (index >= text.length) {
					clearInterval(interval);
				}
			}, speed);
		}, startDelay);

		return () => {
			clearTimeout(timeout);
			clearInterval(interval);
		};
	});
</script>

<span
	class="terminal-typing ml-2 inline-flex align-baseline font-mono text-[0.55em] font-medium text-emerald-600"
	aria-label={`${prefix} ${text}`}
>
	<span aria-hidden="true" class="text-neutral-400">{prefix}&nbsp;</span>
	<span aria-hidden="true">{typed}</span>
	<span aria-hidden="true" class="ml-0.5 inline-block h-[1em] w-[0.08em] bg-emerald-600"></span>
</span>

<style>
	.terminal-typing span:last-child {
		animation: cursor-blink 1s steps(2, start) infinite;
	}

	@keyframes cursor-blink {
		50% {
			opacity: 0;
		}
	}

	@media (prefers-reduced-motion: reduce) {
		.terminal-typing span:last-child {
			animation: none;
		}
	}
</style>
