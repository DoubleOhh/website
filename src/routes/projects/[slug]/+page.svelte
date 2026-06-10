<script lang="ts">
	import { resolve } from '$app/paths';
	import { page } from '$app/state';
	import TerminalTyping from '$lib/components/TerminalTyping.svelte';

	type Project = {
		title: string;
		description: string;
		image: string;
		tools: string[];
		githubUrl?: string;
	};

	const projects: Record<string, Project> = {
		'portfolio-website': {
			title: 'Portfolio Website',
			description: 'An interactive website that shows my projects and cool things I have done',
			image: '/projects/portfolio-preview.svg',
			tools: ['SvelteKit', 'TypeScript', 'Tailwind CSS'],
			githubUrl: 'https://github.com/DoubleOhh/website'
		},
		'mbta-reliability-ridership': {
			title: 'MBTA Line Reliability VS Ridership',
			description:
				"This project explores the relationship between MBTA ridership and system reliability using interactive visualizations. Developed as part of COSI 116A: Information Visualization at Brandeis University, this project uses data from the MBTA's open data portal to help users understand trends and patterns in public transportation performance.",
			image: '/projects/portfolio-preview.svg',
			tools: ['JavaScript', 'D3.js'],
			githubUrl: 'https://github.com/amiefeng/COSI116A-Fall24-Team4'
		},
		lifeleveler: {
			title: 'LifeLeveler',
			description:
				'Built a backend that stores user data and goals using FastAPI, then uses AI to generate sub-goals and actionable steps.',
			image: '/projects/dashboard-preview.svg',
			tools: ['Git', 'Python', 'FastAPI', 'TypeScript', 'Gemini'],
			githubUrl: 'https://github.com/DoubleOhh/lifeleveler'
		},
		'nypd-arrests-analysis': {
			title: 'NYPD Arrests Analysis',
			description:
				'Performed a comprehensive analysis of 150,000+ NYPD arrest records from 2021 using Tableau and Excel to identify high-crime areas, demographic disparities, and systemic inequities. Built three Tableau visualizations, delivered insights through a 10-slide presentation and executive brief, and produced data-driven policy recommendations.',
			image: '/projects/tasks-preview.svg',
			tools: ['Tableau', 'Excel']
		},
		'mfa-lock': {
			title: 'MFA Lock',
			description:
				'Engineered a distributed authentication smart lock using two Raspberry Pi devices, Flask, Socket.IO, serial communication, and subprocess orchestration. The system supports facial, voice, and keypad recognition, a real-time dashboard, touchscreen UI, event logging, and TCP-based lock actuation through servo control.',
			image: '/projects/dashboard-preview.svg',
			tools: ['MicroPython', 'Python', 'Vosk', 'Flask', 'Socket.IO', 'Git'],
			githubUrl: 'https://github.com/jameskong098/mfalock'
		},
		'excel-to-ics': {
			title: 'Excel to iCalendar Converter',
			description:
				'The script allows you to convert a workday schedule from Brandeis to a file used for calendars. ',
			image: '/projects/tasks-preview.svg',
			tools: ['Python'],
			githubUrl: 'https://github.com/DoubleOhh/excel-to-ics'
		}
	};

	let slug = $derived(page.params.slug ?? '');
	let project = $derived(projects[slug]);
</script>

<section class="px-6 py-20">
	<div class="mx-auto max-w-5xl">
		{#if project}
			<a
				href={resolve('/#projects')}
				class="text-sm font-medium text-neutral-600 transition hover:text-black"
			>
				Back to projects
			</a>

			<div class="mt-8 grid gap-10 lg:grid-cols-[1.1fr_0.9fr] lg:items-start">
				<img
					src={project.image}
					alt={`${project.title} preview`}
					class="w-full rounded-2xl border border-neutral-200 object-cover shadow-sm"
				/>

				<div>
					<p class="text-sm font-medium tracking-[0.3em] text-neutral-500 uppercase">Project</p>
					<h1 class="mt-4 text-4xl font-bold tracking-tight text-black sm:text-5xl">
						{project.title}
						<TerminalTyping text={`open ${slug}`} />
					</h1>
					<p class="mt-6 text-lg leading-8 text-neutral-600">
						{project.description}
					</p>

					{#if project.githubUrl}
						<button
							type="button"
							onclick={() => window.open(project.githubUrl, '_blank', 'noreferrer')}
							class="mt-8 inline-flex rounded-full bg-black px-6 py-3 text-sm font-semibold text-white transition hover:bg-neutral-800"
						>
							View on GitHub
						</button>
					{/if}

					<div class="mt-8 flex flex-wrap gap-3">
						{#each project.tools as tool (tool)}
							<span class="rounded-full border border-neutral-300 px-4 py-2 text-sm font-medium">
								{tool}
							</span>
						{/each}
					</div>
				</div>
			</div>
		{:else}
			<div class="max-w-2xl">
				<p class="text-sm font-medium tracking-[0.3em] text-neutral-500 uppercase">Project</p>
				<h1 class="mt-4 text-4xl font-bold tracking-tight text-black">
					Project not found.
					<TerminalTyping text="404" />
				</h1>
				<a
					href={resolve('/#projects')}
					class="mt-8 inline-flex rounded-full bg-black px-6 py-3 text-sm font-semibold text-white transition hover:bg-neutral-800"
				>
					Back to projects
				</a>
			</div>
		{/if}
	</div>
</section>
