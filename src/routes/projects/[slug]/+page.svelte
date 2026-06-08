<script lang="ts">
  import { page } from '$app/state';

  type Project = {
    title: string;
    description: string;
    image: string;
    tools: string[];
  };

  const projects: Record<string, Project> = {
    'portfolio-website': {
      title: 'Portfolio Website',
      description:
        'A responsive SvelteKit site for showcasing projects, skills, and contact details with a clean visual system.',
      image: '/projects/portfolio-preview.svg',
      tools: ['SvelteKit', 'TypeScript', 'Tailwind CSS'],
    },
    'api-dashboard': {
      title: 'API Dashboard',
      description:
        'A clean interface for viewing backend data, request status, and useful metrics in one focused workspace.',
      image: '/projects/dashboard-preview.svg',
      tools: ['FastAPI', 'SQL', 'TypeScript'],
    },
    'task-manager': {
      title: 'Task Manager',
      description:
        'A practical productivity app with organized lists, filters, and simple workflows for tracking work.',
      image: '/projects/tasks-preview.svg',
      tools: ['SvelteKit', 'TypeScript', 'Local State'],
    },
  };

  let slug = $derived(page.params.slug ?? '');
  let project = $derived(projects[slug]);
</script>

<section class="px-6 py-20">
  <div class="mx-auto max-w-5xl">
    {#if project}
      <a href="/#projects" class="text-sm font-medium text-neutral-600 transition hover:text-black">
        Back to projects
      </a>

      <div class="mt-8 grid gap-10 lg:grid-cols-[1.1fr_0.9fr] lg:items-start">
        <img
          src={project.image}
          alt={`${project.title} preview`}
          class="w-full rounded-2xl border border-neutral-200 object-cover shadow-sm"
        />

        <div>
          <p class="text-sm font-medium uppercase tracking-[0.3em] text-neutral-500">
            Project
          </p>
          <h1 class="mt-4 text-4xl font-bold tracking-tight text-black sm:text-5xl">
            {project.title}
          </h1>
          <p class="mt-6 text-lg leading-8 text-neutral-600">
            {project.description}
          </p>

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
        <p class="text-sm font-medium uppercase tracking-[0.3em] text-neutral-500">
          Project
        </p>
        <h1 class="mt-4 text-4xl font-bold tracking-tight text-black">
          Project not found.
        </h1>
        <a
          href="/#projects"
          class="mt-8 inline-flex rounded-full bg-black px-6 py-3 text-sm font-semibold text-white transition hover:bg-neutral-800"
        >
          Back to projects
        </a>
      </div>
    {/if}
  </div>
</section>
