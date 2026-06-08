<script lang="ts">
	import InterestBlock from '$lib/components/InterestBlock.svelte';
	import type { PathnameWithSearchOrHash } from '$app/types';

	type Project = {
		title: string;
		subtitle: string;
		icon: string;
		image: string;
		href: PathnameWithSearchOrHash;
	};

	const projects: Project[] = [
		{
			title: 'Portfolio Website',
			subtitle: 'A responsive SvelteKit site for showcasing projects, skills, and contact details.',
			icon: '⌘',
			image: '/projects/portfolio-preview.svg',
			href: '/projects/portfolio-website'
		},
		{
			title: 'API Dashboard',
			subtitle: 'A clean interface for viewing backend data, request status, and useful metrics.',
			icon: '{}',
			image: '/projects/dashboard-preview.svg',
			href: '/projects/api-dashboard'
		},
		{
			title: 'Task Manager',
			subtitle: 'A practical productivity app with organized lists, filters, and simple workflows.',
			icon: '✓',
			image: '/projects/tasks-preview.svg',
			href: '/projects/task-manager'
		}
	];
</script>

<section id="projects" class="px-6 py-20">
	<div class="mx-auto max-w-5xl">
		<div class="max-w-2xl">
			<p class="text-sm font-medium tracking-[0.3em] text-neutral-500 uppercase">Projects</p>
			<h2 class="mt-4 text-3xl font-bold tracking-tight text-black sm:text-4xl">
				Cool Stuff I am doing/did.
			</h2>
			<p class="mt-5 text-lg leading-8 text-neutral-600">
				A few examples of the kinds of applications I like building: focused, useful, and shaped
				around real user needs.
			</p>
		</div>

		<div class="mt-10 grid gap-5 sm:grid-cols-2 lg:grid-cols-3">
			{#each projects as project (project.title)}
				<InterestBlock
					title={project.title}
					subtitle={project.subtitle}
					icon={project.icon}
					image={project.image}
					href={project.href}
				/>
			{/each}
		</div>
	</div>
</section>
