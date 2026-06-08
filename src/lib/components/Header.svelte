<script lang="ts">
	import { resolve } from '$app/paths';

	type Tab = {
		name: string;
		link: string;
	};

	type Props = {
		y?: number;
		tabs?: Tab[];
	};

	let {
		y = 0,
		tabs = [
			{ name: 'Projects ', link: '/#projects' },
			{ name: 'About me', link: '/#about' }
			//{ name: "Blog", link: "" },
			// { name: "Contact", link: "" },
			// {name: '', link: ''},
		]
	}: Props = $props();

	let isScrolled = $derived(y > 0);
</script>

<header
	class={'sticky top-0 z-[10] flex items-center justify-between border-b border-solid px-6 duration-200 ' +
		(isScrolled
			? ' border-neutral-800 bg-black/90 py-4 text-white backdrop-blur'
			: ' border-transparent bg-transparent py-6')}
>
	<h1 class="font-bold tracking-tight">
		Omorogieva
		<span class={'font-medium ' + (isScrolled ? 'text-neutral-300' : 'text-neutral-600')}>
			Ogieva
		</span>
	</h1>
	<div class="hidden items-center gap-4 sm:flex">
		{#each tabs as tab, index (tab.link)}
			<a
				href={resolve(tab.link)}
				class={'text-sm font-medium transition ' +
					(isScrolled ? 'text-neutral-300 hover:text-white' : 'text-neutral-600 hover:text-black')}
				target={index === 2 ? '_blank' : ''}
			>
				<p>{tab.name}</p>
			</a>
		{/each}
		<!-- <button
            class="blueShadow relative overflow-hidden px-5 py-2 group rounded-full bg-violet-600 text-white"
        >
            <div
                class="absolute top-0 right-full w-full h-full bg-violet-400 opacity-20 group-hover:translate-x-full z-0 duration-200"
            />
            <h4 class="relative z-9">Login</h4>
        </button> -->
		<button
			class={'relative overflow-hidden rounded-full px-5 py-2 text-sm font-semibold transition ' +
				(isScrolled
					? 'bg-white text-black hover:bg-neutral-200'
					: 'bg-black text-white hover:bg-neutral-800')}
		>
			Get in touch
		</button>
	</div>
</header>
