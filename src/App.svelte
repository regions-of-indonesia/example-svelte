<script lang="ts">
  import { onMount } from "svelte";

  import { create } from "@regions-of-indonesia/client";
  import type { Region } from "@regions-of-indonesia/types";

  const client = create();

  let provinces: Region[] = [];
  let districts: Region[] = [];
  let subdistricts: Region[] = [];
  let villages: Region[] = [];

  let selectedProvinceCode: string = "";
  let selectedDistrictCode: string = "";
  let selectedSubdistrictsCode: string = "";
  let selectedVillageCode: string = "";

  onMount(async () => {
    try {
      provinces = await client.province.find();
    } catch (error) {
      provinces = [];
    }
  });

  $: {
    const effect = async () => {
      selectedDistrictCode = "";

      try {
        districts = await client.district.find(selectedProvinceCode);
      } catch (error) {
        districts = [];
      }
    };

    effect();
  }

  $: {
    const effect = async () => {
      selectedSubdistrictsCode = "";

      try {
        subdistricts = await client.subdistrict.find(selectedDistrictCode);
      } catch (error) {
        subdistricts = [];
      }
    };

    effect();
  }

  $: {
    const effect = async () => {
      selectedVillageCode = "";

      try {
        villages = await client.village.find(selectedSubdistrictsCode);
      } catch (error) {
        villages = [];
      }
    };

    effect();
  }
</script>

<div class="container max-w-screen-lg mx-auto p-4 md:p-6 lg:p-8 xl:p-10">
  <h1 class="mb-4 lg:mb-6 text-center text-lg lg:text-xl font-mono">Regions of Indonesia</h1>

  <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-2 sm:gap-3 lg:gap-4">
    <div>
      <select class="select select-bordered select-xs w-full" bind:value={selectedProvinceCode}>
        <option value="" disabled> Select... </option>

        {#each provinces as region}<option value={region.code}>{region.name}</option>{/each}
      </select>
    </div>
    <div>
      <select class="select select-bordered select-xs w-full" bind:value={selectedDistrictCode}>
        <option value="" disabled> Select... </option>

        {#each districts as region}<option value={region.code}>{region.name}</option>{/each}
      </select>
    </div>
    <div>
      <select class="select select-bordered select-xs w-full" bind:value={selectedSubdistrictsCode}>
        <option value="" disabled> Select... </option>

        {#each subdistricts as region}<option value={region.code}>{region.name}</option>{/each}
      </select>
    </div>
    <div>
      <select class="select select-bordered select-xs w-full" bind:value={selectedVillageCode}>
        <option value="" disabled> Select... </option>

        {#each villages as region}<option value={region.code}>{region.name}</option>{/each}
      </select>
    </div>
  </div>
</div>
