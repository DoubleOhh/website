<script lang="ts">
	import { resolve } from '$app/paths';

	type Props = {
		title: string;
		subtitle: string;
		icon?: string;
		image?: string;
		imageAlt?: string;
		href: string;
	};

	let {
		title,
		subtitle,
		icon = '✦',
		image,
		imageAlt = `${title} project preview`,
		href
	}: Props = $props();
</script>

<a
	href={resolve(href)}
	aria-label={`View ${title}`}
	class="group relative block min-h-56 overflow-hidden rounded-2xl border border-neutral-300 bg-white/70 p-5 shadow-sm backdrop-blur transition duration-300 hover:-translate-y-2 hover:rotate-1 hover:shadow-xl focus:ring-2 focus:ring-black focus:ring-offset-4 focus:outline-none"
>
	{#if image}
		<img
			src={image}
			alt={imageAlt}
			class="absolute inset-0 h-full w-full scale-105 object-cover opacity-0 transition duration-300 group-hover:scale-100 group-hover:opacity-100"
		/>
		<div
			class="absolute inset-0 bg-black/55 opacity-0 transition duration-300 group-hover:opacity-100"
		></div>
	{/if}

	<div
		class="relative z-10 mb-4 text-2xl transition duration-300 group-hover:scale-110 group-hover:text-white"
	>
		{icon}
	</div>

	<h3 class="relative z-10 font-semibold transition duration-300 group-hover:text-white">
		{title}
	</h3>
	<p
		class="relative z-10 mt-2 text-sm text-neutral-600 transition duration-300 group-hover:text-neutral-100"
	>
		{subtitle}
	</p>
</a>
