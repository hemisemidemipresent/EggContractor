<template>
  <div class="mx-4 xl:mx-0 my-4">
    <h2 class="mx-4 mt-4 mb-2 text-center text-md leading-6 font-medium text-gray-900">
      Artifacting progress
    </h2>

    <div class="flex justify-center my-2">
      <div class="relative flex items-start">
        <div class="flex items-center h-5">
          <input
            id="spoilers"
            name="spoilers"
            v-model="spoilers"
            type="checkbox"
            class="focus:ring-green-500 h-4 w-4 text-green-600 border-gray-300 rounded"
          />
        </div>
        <div class="ml-2 text-sm">
          <label for="spoilers" class="text-gray-600">Show unseen items (SPOILERS)</label>
        </div>
      </div>
    </div>

    <div
      class="my-2 text-center text-sm text-gray-900"
      v-tippy="{
        content:
          'This estimate may be inaccurate if crafting cost parameters were ever changed server-side.',
      }"
    >
      <span class="inline-flex items-center space-x-1">
        Estimated total crafting expense so far:
        <img class="h-4 w-4" src="https://eggincassets.tcl.sh/64/egginc-extras/icon_golden_egg.png" />
        {{ totalCraftingCost.toLocaleString("en-US") }}
        <svg
          class="h-4 w-4 text-gray-400"
          xmlns="http://www.w3.org/2000/svg"
          viewBox="0 0 20 20"
          fill="currentColor"
          aria-hidden="true"
        >
          <path
            fill-rule="evenodd"
            d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-8-3a1 1 0 00-.867.5 1 1 0 11-1.731-1A3 3 0 0113 8a3.001 3.001 0 01-2 2.83V11a1 1 0 11-2 0v-1a1 1 0 011-1 1 1 0 100-2zm0 8a1 1 0 100-2 1 1 0 000 2z"
            clip-rule="evenodd"
          ></path>
        </svg>
      </span>
    </div>

    <h3 class="my-2 text-sm font-medium text-gray-900">Artifacts</h3>
    <artifact-grid :items="progress.artifacts" :spoilers="spoilers"></artifact-grid>

    <h3 class="my-2 text-sm font-medium text-gray-900">Stones &amp; stone fragments</h3>
    <artifact-grid :items="progress.stones" :spoilers="spoilers"></artifact-grid>

    <h3 class="my-2 text-sm font-medium text-gray-900">Ingredients</h3>
    <artifact-grid :items="progress.ingredients" :spoilers="spoilers"></artifact-grid>
  </div>
</template>

<script>
import ArtifactGrid from "./ArtifactGrid.vue";
import { getLocalStorage, setLocalStorage } from "./utils";

export default {
  components: { ArtifactGrid },

  props: {
    progress: Object,
  },

  computed: {
    totalCraftingCost() {
      const classes = [].concat(
        this.progress.artifacts,
        this.progress.stones,
        this.progress.ingredients
      );
      let sum = 0;
      for (const cls of classes) {
        for (const tier of cls.tiers) {
          sum += tier.craftingCost;
        }
      }
      return sum;
    },
  },

  data() {
    return {
      spoilers: getLocalStorage("spoilers") === "true",
    };
  },

  watch: {
    spoilers() {
      setLocalStorage("spoilers", this.spoilers);
    },
  },
};
</script>
