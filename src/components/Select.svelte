<script lang="ts">
  import clsx from "clsx";

  export let value: string;
  let className: string | undefined = undefined;
  export { className as class };
</script>

<select
  {...$$restProps}
  bind:value
  class={clsx("block w-full bg-neutral-1 text-neutral-12 focus:border-primary-9 rounded-xl focus:ring-primary-9", className)}
>
  <slot />
</select>
