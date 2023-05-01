<template>
  <n-config-provider
    :theme="selectedTheme === 'Dark' ? theme : null"
    :theme-overrides="selectedTheme === 'Dark' ? themeOverrides : null"
  >
    <div>
      <CoalitionComponent :transfer-options="countries" />
    </div>
  </n-config-provider>
</template>

<script lang="ts">
import type { GlobalTheme, GlobalThemeOverrides } from "naive-ui";
import { NConfigProvider } from "naive-ui";
import CoalitionComponent from "./components/CoalitionComponent.vue"
import { useThemeStore } from "./stores/state";
import { ref } from "vue";
export default {
  setup() {
    const themeStore = useThemeStore();
    const theme = ref<GlobalTheme>(themeStore.theme);
    const selectedTheme = ref<string>(themeStore.getSelectedTheme);
    const themeOverrides = ref<GlobalThemeOverrides>(
      themeStore.getThemeOverrides
    );

    const setThemeOverrides = (overrides: GlobalThemeOverrides) => {
      themeStore.setThemeOverrides(overrides);
    };

    const countries = [
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

    return {
      countries,
      theme,
      selectedTheme,
      themeOverrides,
      setThemeOverrides,
    };
  },
  components: {
    CoalitionComponent
  },
};
</script>
