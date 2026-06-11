<script lang="ts">
	import { resolve } from '$app/paths';
	import { page } from '$app/state';
	import TerminalTyping from '$lib/components/TerminalTyping.svelte';

	type Project = {
		title: string;
		description: string;
		details?: string[];
		image: string;
		images?: string[];
		tools: string[];
		githubUrl?: string;
	};

	const visibleToolCount = 4;

	const projects: Record<string, Project> = {
		'portfolio-website': {
			title: 'Portfolio Website',
			description: 'An interactive website that shows my projects and cool things I have done',
			details: [
				'Designed and deployed a production-style portfolio website using SvelteKit and AWS, with a focus on cloud infrastructure, automation, and Infrastructure as Code principles.',
				'The frontend was built with SvelteKit and deployed as a static site to Amazon S3. To improve performance, security, and scalability, the site is served globally through Amazon CloudFront, providing HTTPS encryption, edge caching, and low-latency content delivery. A custom domain was configured using DNS records and AWS Certificate Manager (ACM) to enable secure access over TLS.',
				'The deployment process is fully automated through GitHub Actions. Every push to the release branch triggers a CI/CD pipeline that installs dependencies, builds the SvelteKit application, synchronizes the generated static assets to Amazon S3, and invalidates the CloudFront cache to ensure visitors always receive the latest version of the site.',
				'To follow AWS security best practices, the deployment workflow uses GitHub OpenID Connect (OIDC) federation to assume an IAM role at deployment time. This eliminates the need for long-lived AWS access keys and provides short-lived, least-privilege credentials during each workflow run.',
				'Infrastructure components, including CloudFront configuration, bucket policies, IAM trust policies, and deployment permissions, are maintained in source control to document and reproduce the environment. The project demonstrates practical experience with AWS CLI, IAM, S3, CloudFront, ACM, DNS management, CI/CD pipelines, GitHub Actions, and cloud security best practices.'
			],
			image: '/projects/portfolio-preview.svg',
			tools: [
				'SvelteKit',
				'TypeScript',
				'AWS S3',
				'AWS CloudFront',
				'AWS Certificate Manager',
				'AWS IAM',
				'GitHub Actions',
				'GitHub OIDC Federation',
				'AWS CLI',
				'DNS Management',
				'Infrastructure as Code Concepts'
			],
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
	let projectImages = $derived(project ? (project.images ?? [project.image]) : []);
	let showAllTools = $state(false);
	let visibleTools = $derived(
		project ? (showAllTools ? project.tools : project.tools.slice(0, visibleToolCount)) : []
	);
	let hiddenToolCount = $derived(project ? project.tools.length - visibleTools.length : 0);
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
				<div class="grid gap-4">
					{#each projectImages as image, index (image)}
						<img
							src={image}
							alt={`${project.title} preview ${index + 1}`}
							class="w-full rounded-2xl border border-neutral-200 object-cover shadow-sm"
						/>
					{/each}
				</div>

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

					<div class="mt-8">
						<p class="text-sm font-semibold text-black">Technologies Used</p>
						<div class="mt-3 flex flex-wrap gap-2">
							{#each visibleTools as tool (tool)}
								<span
									class="rounded-full border border-neutral-300 px-3 py-1.5 text-sm font-medium"
								>
									{tool}
								</span>
							{/each}
						</div>

						{#if project.tools.length > visibleToolCount}
							<button
								type="button"
								onclick={() => (showAllTools = !showAllTools)}
								class="mt-3 text-sm font-semibold text-neutral-600 transition hover:text-black"
							>
								{showAllTools ? 'Show less' : `Show ${hiddenToolCount} more`}
							</button>
						{/if}
					</div>
				</div>
			</div>

			{#if project.details?.length}
				<div class="mt-16 border-t border-neutral-200 pt-12">
					<p class="text-sm font-medium tracking-[0.3em] text-neutral-500 uppercase">
						Project Deep Dive
					</p>
					<div class="mt-6 max-w-3xl space-y-5 text-lg leading-8 text-neutral-600">
						{#each project.details as paragraph (paragraph)}
							<p>{paragraph}</p>
						{/each}
					</div>
				</div>
			{/if}
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
