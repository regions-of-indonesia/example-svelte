<script lang="ts">
  import clsx from "clsx";

  let className: string | undefined = undefined;
  export { className as class };
</script>

<label {...$$restProps} class={clsx("text-neutral-11 text-base font-medium", className)}>
  <slot />
</label>
