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
			subtitle: 'An interactive website that shows my projects and cool things I have done.',
			icon: '⌘',
			image: '/projects/portfolio-preview.svg',
			href: '/projects/portfolio-website'
		},
		{
			title: 'MBTA Line Reliability VS Ridership',
			subtitle:
				'Interactive visualizations exploring the relationship between MBTA ridership and system reliability.',
			icon: 'D3',
			image: '/projects/portfolio-preview.svg',
			href: '/projects/mbta-reliability-ridership'
		},
		{
			title: 'LifeLeveler',
			subtitle: 'A goal-planning backend that uses AI to break big goals into actionable steps.',
			icon: 'AI',
			image: '/projects/dashboard-preview.svg',
			href: '/projects/lifeleveler'
		},
		{
			title: 'NYPD Arrests Analysis',
			subtitle:
				'Tableau and Excel analysis of 150,000+ NYPD arrest records to surface policing patterns.',
			icon: 'NYC',
			image: '/projects/tasks-preview.svg',
			href: '/projects/nypd-arrests-analysis'
		},
		{
			title: 'MFA Lock',
			subtitle:
				'A distributed multi-factor smart lock using Raspberry Pi, Flask, Socket.IO, and sensors.',
			icon: '🔒',
			image: '/projects/dashboard-preview.svg',
			href: '/projects/mfa-lock'
		},
		{
			title: 'Excel to iCalendar Converter',
			subtitle: 'A Python script that converts a Brandeis Workday schedule into a calendar file.',
			icon: 'ICS',
			image: '/projects/tasks-preview.svg',
			href: '/projects/excel-to-ics'
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
