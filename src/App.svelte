<script lang="ts">
  import { onMount } from "svelte";

  import { create, cache } from "@regions-of-indonesia/client";
  import { isRegionCode } from "@regions-of-indonesia/utils";
  import type { Region } from "@regions-of-indonesia/types";

  import Label from "./components/Label.svelte";
  import Select from "./components/Select.svelte";
  import RegionSelectOptions from "./components/RegionSelectOptions.svelte";

  const client = create({
    middlewares: [cache()],
  });

  const parseRegionCode = (value: unknown) => {
    if (value && isRegionCode(value)) return value;
    throw new Error("Invalid region code");
  };

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
      console.error(error);
      provinces = [];
    }
  });

  $: {
    const effect = async () => {
      selectedDistrictCode = "";

      try {
        districts = await client.district.find(parseRegionCode(selectedProvinceCode));
      } catch (error) {
        console.error(error);
        districts = [];
      }
    };

    effect();
  }

  $: {
    const effect = async () => {
      selectedSubdistrictsCode = "";

      try {
        subdistricts = await client.subdistrict.find(parseRegionCode(selectedDistrictCode));
      } catch (error) {
        console.error(error);
        subdistricts = [];
      }
    };

    effect();
  }

  $: {
    const effect = async () => {
      selectedVillageCode = "";

      try {
        villages = await client.village.find(parseRegionCode(selectedSubdistrictsCode));
      } catch (error) {
        console.error(error);
        villages = [];
      }
    };

    effect();
  }
</script>

<div class="container max-w-screen-lg mx-auto p-4 md:p-6 lg:p-8 xl:p-10">
  <h1 class="mb-4 lg:mb-6 text-center text-lg md:text-xl 2xl:text-2xl font-mono font-bold">Regions of Indonesia</h1>

  <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-2 sm:gap-3 lg:gap-4">
    <div class="flex flex-col gap-1">
      <Label for="select-provinces">Provinces</Label>
      <Select id="select-provinces" bind:value={selectedProvinceCode}>
        <option value="" disabled> Select... </option>
        <RegionSelectOptions regions={provinces} />
      </Select>
    </div>
    <div class="flex flex-col gap-1">
      <Label for="select-districts">Districts</Label>
      <Select id="select-districts" bind:value={selectedDistrictCode}>
        <option value="" disabled> Select... </option>
        <RegionSelectOptions regions={districts} />
      </Select>
    </div>
    <div class="flex flex-col gap-1">
      <Label for="select-subdistricts">Subdistricts</Label>
      <Select id="select-subdistricts" bind:value={selectedSubdistrictsCode}>
        <option value="" disabled> Select... </option>
        <RegionSelectOptions regions={subdistricts} />
      </Select>
    </div>
    <div class="flex flex-col gap-1">
      <Label for="select-villages">Villages</Label>
      <Select id="select-villages" bind:value={selectedVillageCode}>
        <option value="" disabled> Select... </option>
        <RegionSelectOptions regions={villages} />
      </Select>
    </div>
  </div>
</div>
