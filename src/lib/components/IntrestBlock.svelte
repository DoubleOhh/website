 <script lang="ts">
  type Props = {
    title: string;
    subtitle: string;
    icon?: string;
  };

  let { title, subtitle, icon = '✦' }: Props = $props();
</script>

<div
  class="group rounded-2xl border border-neutral-300 bg-white/70 p-5 shadow-sm backdrop-blur transition duration-300 hover:-translate-y-2 hover:rotate-1 hover:shadow-xl"
>
  <div class="mb-4 text-2xl transition duration-300 group-hover:scale-110">
    {icon}
  </div>

  <h3 class="font-semibold">{title}</h3>
  <p class="mt-2 text-sm text-neutral-600">{subtitle}</p>
</div>