<template>
  <div class="w-full h-full">
    <div class="flex flex-row justify-center p-5 relative">
      <n-select
        v-model:value="preset"
        :options="preset_options"
        class="absolute left-0 ml-5 w-1/4"
      ></n-select>
      <button @click="handleLeftArrowClick">
        <n-icon size="35">
          <img src="../assets/leftarrow.svg" />
        </n-icon>
      </button>
      <button @click="handleCircleClick">
        <n-icon size="35">
          <img src="../assets/circle.svg" />
        </n-icon>
      </button>
      <button @click="handleRightArrowClick">
        <n-icon size="35">
          <img src="../assets/rightarrow.svg" />
        </n-icon>
      </button>
    </div>
    <div class="m-5">
      <div class="flex">
        <div class="w-1/2 mr-4">
          <h3 class="mb-3 text-lg font-semibold">Red</h3>
          <ul class="list-none border border-gray-300 p-6 pl-4 rounded-lg">
            <li
              v-for="(option, index) in sorted_red"
              :key="index"
              class="mb-2 cursor-pointer rounded"
              :class="{
                'bg-blue-200 pl-2 text-black':
                  currentSelection.list === 'red' &&
                  currentSelection.index === index,
              }"
              @click="handleItemClick('red', index)"
            >
              {{ findCountryByValue(option) }}
            </li>
          </ul>
        </div>
        <div class="w-1/3 mr-4">
          <h3 class="mb-3 text-lg font-semibold">Neutral</h3>
          <ul class="list-none border border-gray-300 p-6 rounded-lg">
            <li
              v-for="(option, index) in sorted_neutral"
              :key="index"
              class="mb-2 cursor-pointer rounded"
              :class="{
                'bg-blue-200 pl-2 text-black':
                  currentSelection.list === 'neutral' &&
                  currentSelection.index === index,
              }"
              @click="handleItemClick('neutral', index)"
            >
              {{ findCountryByValue(option) }}
            </li>
          </ul>
        </div>
        <div class="w-1/2">
          <h3 class="mb-3 text-lg font-semibold">Blue</h3>
          <ul class="list-none border border-gray-300 p-6 rounded-lg">
            <li
              v-for="(option, index) in sorted_blue"
              :key="index"
              class="mb-2 cursor-pointer rounded"
              :class="{
                'bg-blue-200 pl-2 text-black':
                  currentSelection.list === 'blue' &&
                  currentSelection.index === index,
              }"
              @click="handleItemClick('blue', index)"
            >
              {{ findCountryByValue(option) }}
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { ref, Ref, computed, defineComponent, watch } from "vue";
import { NIcon, NSelect } from "naive-ui";
import { countries } from "../libs/lib";
import { useCoalitionStore } from "../stores/state";

export default defineComponent({
  setup() {
    const coaStore = useCoalitionStore();

    const [blue, neutral, red] = Object.values(coaStore.coa.coalitions).map(
      (v) => ref<number[]>(v)
    );

    const currentSelection = ref<{ list: string; index: number }>({
      list: "red",
      index: 0,
    });

    const handleItemClick = (list: string, index: number) => {
      currentSelection.value.list = list;
      currentSelection.value.index = index;
    };

    // Code is pretty ugly, will refactor later

    const moveItem = (fromList: string, toList: string, condition: boolean) => {
      if (condition) {
        const from =
          fromList === "red" ? red : fromList === "blue" ? blue : neutral;
        const to = toList === "red" ? red : toList === "blue" ? blue : neutral;
        const sortedFrom =
          fromList === "red"
            ? sorted_red
            : fromList === "blue"
            ? sorted_blue
            : sorted_neutral;
        const selectedItem = sortedFrom.value[currentSelection.value.index];
        const originalIndex = from.value.indexOf(selectedItem);
        const option = from.value.splice(originalIndex, 1)[0];
        to.value.push(option);
        currentSelection.value.list = toList;
        currentSelection.value.index = -1;
      }
    };

    const handleLeftArrowClick = () => {
      moveItem(
        "blue",
        "red",
        currentSelection.value.list === "blue" &&
          currentSelection.value.index >= 0 &&
          blue.value.length > 0
      );
      moveItem(
        "neutral",
        "red",
        currentSelection.value.list === "neutral" &&
          currentSelection.value.index >= 0 &&
          neutral.value.length > 0
      );
    };

    const handleRightArrowClick = () => {
      moveItem(
        "red",
        "blue",
        currentSelection.value.list === "red" &&
          currentSelection.value.index >= 0 &&
          red.value.length > 0
      );
      moveItem(
        "neutral",
        "blue",
        currentSelection.value.list === "neutral" &&
          currentSelection.value.index >= 0 &&
          neutral.value.length > 0
      );
    };

    const handleCircleClick = () => {
      moveItem(
        "red",
        "neutral",
        currentSelection.value.list === "red" &&
          currentSelection.value.index >= 0 &&
          red.value.length > 0
      );
      moveItem(
        "blue",
        "neutral",
        currentSelection.value.list === "blue" &&
          currentSelection.value.index >= 0 &&
          blue.value.length > 0
      );
    };

    const sorted = (src: Ref<number[]>) => {
      return src.value.slice().sort((a, b) => a - b);
    };

    const sorted_red = computed(() => sorted(red));
    const sorted_neutral = computed(() => sorted(neutral));
    const sorted_blue = computed(() => sorted(blue));

    watch(sorted_red, (val) => {
      coaStore.coa.coalitions.red = val;
    });

    watch(sorted_blue, (val) => {
      coaStore.coa.coalitions.blue = val;
    });

    watch(sorted_neutral, (val) => {
      coaStore.coa.coalitions.neutrals = val;
    });

    const findCountryByValue = (value: number): string | null => {
      const country = countries.find((country) => country.value === value);
      return country ? country.label : null;
    };

    const preset = ref("Custom");

    const preset_options = [
      { label: "Modern", value: "Modern" },
      { label: "Cold War 1947-1991", value: "ColdWar" },
      { label: "WWII", value: "WW2" },
      { label: "Custom", value: "Custom" },
    ];

    return {
      sorted_red,
      sorted_blue,
      sorted_neutral,
      red,
      blue,
      neutral,
      currentSelection,
      preset_options,
      preset,
      findCountryByValue,
      handleLeftArrowClick,
      handleRightArrowClick,
      handleCircleClick,
      handleItemClick,
    };
  },
  components: {
    NSelect,
    NIcon,
  },
});
</script>
