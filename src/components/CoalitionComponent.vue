<template>
  <div class="w-full h-full">
    <div class="flex flex-row justify-center p-5">
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
              v-for="(option, index) in sortedSource"
              :key="index"
              class="mb-2 cursor-pointer rounded"
              :class="{
                'bg-blue-200 pl-2 text-black': currentSourceIndex === index,
              }"
              @click="
                currentSourceIndex = index;
                currentTargetIndex = -1;
                currentNeutralIndex = -1;
              "
            >
              {{ option.label }}
            </li>
          </ul>
        </div>
        <div class="w-1/3 mr-4">
          <h3 class="mb-3 text-lg font-semibold">Neutral</h3>
          <ul class="list-none border border-gray-300 p-6 rounded-lg">
            <li
              v-for="(option, index) in sortedNeutral"
              :key="index"
              class="mb-2 cursor-pointer rounded"
              :class="{
                'bg-blue-200 pl-2 text-black': currentNeutralIndex === index,
              }"
              @click="
                currentNeutralIndex = index;
                currentSourceIndex = -1;
                currentTargetIndex = -1;
              "
            >
              {{ option.label }}
            </li>
          </ul>
        </div>
        <div class="w-1/2">
          <h3 class="mb-3 text-lg font-semibold">Blue</h3>
          <ul class="list-none border border-gray-300 p-6 rounded-lg">
            <li
              v-for="(option, index) in sortedTarget"
              :key="index"
              class="mb-2 cursor-pointer rounded"
              :class="{
                'bg-blue-200 pl-2 text-black': currentTargetIndex === index,
              }"
              @click="
                currentTargetIndex = index;
                currentSourceIndex = -1;
                currentNeutralIndex = -1;
              "
            >
              {{ option.label }}
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { ref, computed, defineComponent } from "vue";
import { NIcon, NH2 } from "naive-ui";

interface Options {
  label: string;
  value: number;
}

const transferOptions: Options[] = [
  { label: "Russia", value: 0 },
  { label: "Ukraine", value: 1 },
  { label: "USA", value: 2 },
  { label: "Turkey", value: 3 },
  { label: "UK", value: 4 },
  { label: "France", value: 5 },
  { label: "Germany", value: 6 },
  { label: "USAF Aggressors", value: 7 },
  { label: "Canada", value: 8 },
  { label: "Spain", value: 9 },
  { label: "The Netherlands", value: 10 },
  { label: "Belgium", value: 11 },
  { label: "Norway", value: 12 },
  { label: "Denmark", value: 13 },
  { label: "Israel", value: 15 },
  { label: "Georgia", value: 16 },
  { label: "Insurgents", value: 17 },
  { label: "Abkhazia", value: 18 },
  { label: "South Osetia", value: 19 },
  { label: "Italy", value: 20 },
  { label: "Australia", value: 21 },
  { label: "Switzerland", value: 22 },
  { label: "Austria", value: 23 },
  { label: "Belarus", value: 24 },
  { label: "Bulgaria", value: 25 },
  { label: "Czech Republic", value: 26 },
  { label: "China", value: 27 },
  { label: "Croatia", value: 28 },
  { label: "Egypt", value: 29 },
  { label: "Finland", value: 30 },
  { label: "Greece", value: 31 },
  { label: "Hungary", value: 32 },
  { label: "India", value: 33 },
  { label: "Iran", value: 34 },
  { label: "Iraq", value: 35 },
  { label: "Japan", value: 36 },
  { label: "Kazakhstan", value: 37 },
  { label: "North Korea", value: 38 },
  { label: "Pakistan", value: 39 },
  { label: "Poland", value: 40 },
  { label: "Romania", value: 41 },
  { label: "Saudi Arabia", value: 42 },
  { label: "Serbia", value: 43 },
  { label: "Slovakia", value: 44 },
  { label: "South Korea", value: 45 },
  { label: "Sweden", value: 46 },
  { label: "Syria", value: 47 },
  { label: "Yemen", value: 48 },
  { label: "Vietnam", value: 49 },
  { label: "Venezuela", value: 50 },
  { label: "Tunisia", value: 51 },
  { label: "Thailand", value: 52 },
  { label: "Sudan", value: 53 },
  { label: "Philippines", value: 54 },
  { label: "Morocco", value: 55 },
  { label: "Mexico", value: 56 },
  { label: "Malaysia", value: 57 },
  { label: "Libya", value: 58 },
  { label: "Jordan", value: 59 },
  { label: "Indonesia", value: 60 },
  { label: "Honduras", value: 61 },
  { label: "Ethiopia", value: 62 },
  { label: "Chile", value: 63 },
  { label: "Brazil", value: 64 },
  { label: "Bahrain", value: 65 },
  { label: "Third Reich", value: 66 },
  { label: "Yugoslavia", value: 67 },
  { label: "USSR", value: 68 },
  { label: "Italian Social Republic", value: 69 },
  { label: "Algeria", value: 70 },
  { label: "Kuwait", value: 71 },
  { label: "Qatar", value: 72 },
  { label: "Oman", value: 73 },
  { label: "United Arab Emirates", value: 74 },
  { label: "South Africa", value: 75 },
  { label: "Cuba", value: 76 },
  { label: "Portugal", value: 77 },
  { label: "GDR", value: 78 },
  { label: "Lebanon", value: 79 },
  { label: "CJTF Blue", value: 80 },
  { label: "CJTF Red", value: 81 },
  { label: "UN", value: 82 },
  { label: "Argentina", value: 83 },
  { label: "Cyprus", value: 84 },
  { label: "Slovenia", value: 85 },
  { label: "Bolivia", value: 86 },
  { label: "Ghana", value: 87 },
  { label: "Nigeria", value: 88 },
  { label: "Peru", value: 89 },
  { label: "Ecuador", value: 90 },
];

const targetOptions: Options[] = [];
const neutralOptions: Options[] = [];

export default defineComponent({
  setup() {
    const source = ref<Options[]>(transferOptions);
    const target = ref<Options[]>(targetOptions);
    const neutral = ref<Options[]>(neutralOptions);
    const currentSourceIndex = ref<number>(0);
    const currentTargetIndex = ref<number>(-1);
    const currentNeutralIndex = ref<number>(-1);

    const handleLeftArrowClick = () => {
      if (currentTargetIndex.value >= 0 && target.value.length > 0) {
        const option = target.value.splice(currentTargetIndex.value, 1)[0];
        source.value.push(option);
        currentTargetIndex.value = -1;
      }
    };

    const handleCircleClick = () => {
      if (currentSourceIndex.value >= 0 && source.value.length > 0) {
        const option = source.value.splice(currentSourceIndex.value, 1)[0];
        neutral.value.push(option);
        currentSourceIndex.value = -1;
      } else if (currentTargetIndex.value >= 0 && target.value.length > 0) {
        const option = target.value.splice(currentTargetIndex.value, 1)[0];
        neutral.value.push(option);
        currentTargetIndex.value = -1;
      }
    };

    const handleRightArrowClick = () => {
      if (currentSourceIndex.value >= 0 && source.value.length > 0) {
        const option = source.value.splice(currentSourceIndex.value, 1)[0];
        target.value.push(option);
        currentSourceIndex.value = -1;
      }
    };

    const sortedSource = computed(() => {
      return source.value.slice().sort((a, b) => a.value - b.value);
    });

    const sortedNeutral = computed(() => {
      return neutral.value.slice().sort((a, b) => a.value - b.value);
    });

    const sortedTarget = computed(() => {
      return target.value.slice().sort((a, b) => a.value - b.value);
    });

    return {
      sortedSource,
      sortedTarget,
      sortedNeutral,
      source,
      target,
      neutral,
      currentSourceIndex,
      currentTargetIndex,
      currentNeutralIndex,
      handleLeftArrowClick,
      handleRightArrowClick,
      handleCircleClick,
    };
  },
  components: {
    NIcon,
    NH2,
  },
});
</script>
