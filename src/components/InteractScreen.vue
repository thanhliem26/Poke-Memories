<template>
  <div class="screen">
    <div
      class="screen__inner"
      :style="{
        width: `${
          ((((heightWd - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4 +
            16) *
          Math.sqrt(cardsContext.length)
        }px`,
      }"
    >
      <card-flip
        v-for="(card, index) in cardsContext"
        :key="index"
        :imgBackFaceUrl="`images/${card}.png`"
        :card="{ index, value: card }"
        @onFlip="checkRule($event)"
        :ref="`card-${index}`"
        :rules="rules"
        :cardsContext="cardsContext"
      />
    </div>
  </div>
</template>

<script>
import CardFlip from "./Card.vue";

export default {
  data() {
    return {
      rules: [],
      widthWd: screen.width - 160,
      heightWd: screen.height - 160,
    };
  },
  methods: {
    checkRule(card) {
      this.rules.push(card);

      if (
        this.rules.length === 2 &&
        this.rules[0].value === this.rules[1].value
      ) {
        this.$refs[`card-${this.rules[0].index}`][0].onEnabledCard();
        this.$refs[`card-${this.rules[1].index}`][0].onEnabledCard();
        this.rules = [];

        const disabledElements = document.querySelectorAll(".card.disabled");

        if (
          disabledElements &&
          disabledElements.length === this.cardsContext.length - 2
        ) {
          setTimeout(() => {
            this.$emit("onFinish");
          }, 1000);
        }
      } else if (
        this.rules.length === 2 &&
        this.rules[0].value !== this.rules[1].value
      ) {
        setTimeout(() => {
          this.$refs[`card-${this.rules[0].index}`][0].onFlipBackCard();
          this.$refs[`card-${this.rules[1].index}`][0].onFlipBackCard();
          this.rules = [];
        }, 800);
      } else {
        return false;
      }
    },
  },
  props: {
    cardsContext: {
      type: Array,
      default: function () {
        return [];
      },
    },
  },
  components: { CardFlip },
};
</script>

<style lang="css" scoped>
.screen {
  width: 100%;
  height: 100%;
  position: absolute;
  inset: 0;
  z-index: 2;
  background-color: var(--dark);
}

.screen__inner {
  display: flex;
  flex-wrap: wrap;
  margin: 2rem auto;
}
</style>
