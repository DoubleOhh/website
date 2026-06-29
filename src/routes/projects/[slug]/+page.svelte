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
		media?: ProjectMedia[];
		tools: string[];
		githubUrl?: string;
		slidesUrl?: string;
	};

	type ProjectMedia = {
		type: 'image' | 'video' | 'youtube';
		src: string;
		alt?: string;
		poster?: string;
		title?: string;
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
			media: [
				{
					type: 'image',
					src: '/projects/portfolio-preview.svg',
					alt: 'Portfolio website homepage preview'
				},
				{
					type: 'image',
					src: '/projects/dashboard-preview.svg',
					alt: 'Portfolio website project page preview'
				},
				{
					type: 'image',
					src: '/projects/tasks-preview.svg',
					alt: 'Portfolio website deployment preview'
				}
			],
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
			media: [
				{
					type: 'image',
					src: '/projects/portfolio-preview.svg',
					alt: 'MBTA reliability and ridership project preview'
				},
				{
					type: 'video',
					src: '/projects/mbta-reliability-ridership/demo-video.mp4',
					poster: '/projects/portfolio-preview.svg',
					title: 'MBTA reliability and ridership demo video'
				}
			],
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
			media: [
				...Array.from({ length: 19 }, (_, index) => ({
					type: 'image' as const,
					src: `/projects/mfa-lock/slide-${String(index + 1).padStart(2, '0')}.png`,
					alt: `MFA Lock project slide ${index + 1}`
				})),
				{
					type: 'youtube',
					src: 'https://www.youtube.com/embed/bCD5fVvEwWU',
					title: 'MFA Lock project demo video'
				}
			],
			tools: ['MicroPython', 'Python', 'Vosk', 'Flask', 'Socket.IO', 'Git'],
			githubUrl: 'https://github.com/jameskong098/mfalock',
			slidesUrl: '/projects/mfa-lock/mfa-lock-project.pdf'
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
	let projectMedia = $derived(
		project
			? (project.media ??
					project.images?.map((src, index) => ({
						type: 'image' as const,
						src,
						alt: `${project.title} preview ${index + 1}`
					})) ?? [
						{
							type: 'image' as const,
							src: project.image,
							alt: `${project.title} preview`
						}
					])
			: []
	);
	let activeMediaIndex = $state(0);
	let activeMedia = $derived(projectMedia[activeMediaIndex] ?? projectMedia[0]);
	let showAllTools = $state(false);
	let visibleTools = $derived(
		project ? (showAllTools ? project.tools : project.tools.slice(0, visibleToolCount)) : []
	);
	let hiddenToolCount = $derived(project ? project.tools.length - visibleTools.length : 0);

	$effect(() => {
		slug;
		activeMediaIndex = 0;
		showAllTools = false;
	});

	function showPreviousMedia() {
		if (!projectMedia.length) return;

		activeMediaIndex = (activeMediaIndex - 1 + projectMedia.length) % projectMedia.length;
	}

	function showNextMedia() {
		if (!projectMedia.length) return;

		activeMediaIndex = (activeMediaIndex + 1) % projectMedia.length;
	}
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
					<div
						class="overflow-hidden rounded-2xl border border-neutral-200 bg-neutral-50 shadow-sm"
					>
						{#if activeMedia?.type === 'video'}
							<video
								src={activeMedia.src}
								poster={activeMedia.poster}
								controls
								playsinline
								class="aspect-video w-full bg-black object-contain"
							>
								<track kind="captions" />
							</video>
						{:else if activeMedia?.type === 'youtube'}
							<iframe
								src={activeMedia.src}
								title={activeMedia.title ?? `${project.title} video`}
								allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
								allowfullscreen
								class="aspect-video w-full border-0 bg-black"
							></iframe>
						{:else if activeMedia}
							<img
								src={activeMedia.src}
								alt={activeMedia.alt ?? `${project.title} preview ${activeMediaIndex + 1}`}
								class="aspect-video w-full object-contain"
							/>
						{/if}
					</div>

					{#if projectMedia.length > 1}
						<div class="flex items-center justify-between gap-3">
							<button
								type="button"
								onclick={showPreviousMedia}
								class="rounded-full border border-neutral-300 px-4 py-2 text-sm font-semibold transition hover:border-black hover:text-black"
							>
								Previous
							</button>
							<p class="text-sm font-medium text-neutral-500">
								{activeMediaIndex + 1} / {projectMedia.length}
							</p>
							<button
								type="button"
								onclick={showNextMedia}
								class="rounded-full border border-neutral-300 px-4 py-2 text-sm font-semibold transition hover:border-black hover:text-black"
							>
								Next
							</button>
						</div>

						<div class="grid grid-cols-4 gap-3 sm:grid-cols-5">
							{#each projectMedia as media, index (media.src)}
								<button
									type="button"
									onclick={() => (activeMediaIndex = index)}
									aria-label={`Show ${project.title} media ${index + 1}`}
									aria-current={activeMediaIndex === index}
									class="aspect-video overflow-hidden rounded-lg border bg-neutral-50 text-xs font-semibold text-neutral-500 transition {activeMediaIndex ===
									index
										? 'border-black'
										: 'border-neutral-200 hover:border-neutral-400'}"
								>
									{#if media.type === 'video'}
										{#if media.poster}
											<img src={media.poster} alt="" class="h-full w-full object-cover" />
										{:else}
											<span class="grid h-full place-items-center">Video {index + 1}</span>
										{/if}
									{:else if media.type === 'youtube'}
										<span class="grid h-full place-items-center bg-black text-white"> Video </span>
									{:else}
										<img src={media.src} alt="" class="h-full w-full object-cover" />
									{/if}
								</button>
							{/each}
						</div>
					{/if}
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

					{#if project.githubUrl || project.slidesUrl}
						<div class="mt-8 flex flex-wrap gap-3">
							{#if project.githubUrl}
								<button
									type="button"
									onclick={() => window.open(project.githubUrl, '_blank', 'noreferrer')}
									class="inline-flex rounded-full bg-black px-6 py-3 text-sm font-semibold text-white transition hover:bg-neutral-800"
								>
									View on GitHub
								</button>
							{/if}

							{#if project.slidesUrl}
								<button
									type="button"
									onclick={() => window.open(project.slidesUrl, '_blank', 'noreferrer')}
									class="inline-flex rounded-full border border-neutral-300 px-6 py-3 text-sm font-semibold transition hover:border-black hover:text-black"
								>
									View Slides
								</button>
							{/if}
						</div>
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
