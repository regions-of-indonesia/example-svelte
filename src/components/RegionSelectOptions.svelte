<script lang="ts">
  import type { Region } from "@regions-of-indonesia/types";

  export let regions: Region[];
</script>

{#each regions as region}<option value={region.code}>{region.name}</option>{/each}
