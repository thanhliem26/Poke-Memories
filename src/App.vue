<template>
  <main-scrren
    @onStart="onHandleBeforeStart($event)"
    v-if="statusMatch === 'default'"
  />
  <interact-screen
    :cardsContext="settings.cardsContext"
    @onFinish="onGetResult()"
    v-if="statusMatch === 'match'"
  />
  <result-screen @startAgain="statusMatch = 'default'" :timer="timer" v-if="statusMatch === 'result'"/>
  <copy-right />
</template>

<script>

import MainScrren from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import ResultScreen from './components/ResultScreen.vue';
import CopyRight from './components/CoppRightScreen.vue';

import { shuffled } from "./utils/array";

export default {
  name: "App",
  components: {
    MainScrren,
    InteractScreen,
    ResultScreen,
    CopyRight,
  },
  data() {
    return {
      settings: {
        totalOfBlocks: 0,
        cardsContext: [],
        startedAt: null,
      },
      statusMatch: "default",
      timer: 0,
    };
  },
  methods: {
    onHandleBeforeStart(config) {
      this.settings.totalOfBlocks = config.totalOfBlocks;

      const firstCard = Array.from(
        { length: this.settings.totalOfBlocks / 2 },
        (_, i) => i + 1
      );
      const secondCard = [...firstCard];
      const cards = [...firstCard, ...secondCard];

      this.settings.cardsContext = shuffled(
        shuffled(shuffled(shuffled(cards)))
      );
      this.settings.startedAt = new Date().getTime();

      this.statusMatch = "match";
    },
    onGetResult() {
      this.timer = new Date().getTime() - this.settings.startedAt;
      this.statusMatch = "result";
    }
  },
};
</script>
