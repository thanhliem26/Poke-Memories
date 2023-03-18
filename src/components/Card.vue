<template>
  <div class="card" :class="{'disabled': isDisabled}" :style="{
    height: `${(heightWd - 16 * 4) / Math.sqrt(cardsContext.length) - 16}px`,
    width: `${((heightWd - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3 / 4}px`,
    perspective: `${((heightWd - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3 / 4 * 2}px`,
  }">
    <div
      class="card__inner"
      @click="onToggleFlipCard"
      :class="{ 'is-flipped': isFlipped }"
    >
      <div class="card__face card__face-front">
        <div class="card__content" :style="{
          backgroundSize: `${((heightWd - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3 / 4 / 3}px`,
        }"></div>
      </div>
      <div class="card__face card__face-back">
        <div
          class="card__content"
          :style="{
            backgroundImage: `url(${require('@/assets/' + imgBackFaceUrl)})`,
            backgroundSize: `${((heightWd - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3 / 4 / 3}px`,
          }"
        ></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    imgBackFaceUrl: {
      type: String,
      required: true,
    },
    card: {
        type: [String, Number, Array, Object], 
    },
    rules: {
      type: Object
    },
    cardsContext: {
      type: Array,
      default: () => [],
    }
  },
  data() {
    return {
      widthWd: screen.width - 160,
      heightWd: screen.height - 160,
      isDisabled: false,
      isFlipped: false,
    };
  },
  methods: {
    onToggleFlipCard() {
      if(this.isDisabled || this.rules.length === 2) return false;
      this.isFlipped = !this.isFlipped;
      if(this.isFlipped) this.$emit("onFlip", this.card);
    },
    onFlipBackCard() {
      this.isFlipped = false;
    },
    onEnabledCard() {
      this.isDisabled = true;
    }
  },
};
</script>

<style lang="css" scoped>
.card {
  display: inline-block;
  margin-right: 1rem;
  margin-bottom: 1rem;
}

.card__inner {
  width: 100%;
  height: 100%;
  transition: transform 1s;
  transform-style: preserve-3d;
  cursor: pointer;
  position: relative;
}

.card.disabled .card__inner {
  cursor: default;
}

.card__inner.is-flipped {
  transform: rotateY(-180deg);
}

.card__face {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  overflow: hidden;
  border-radius: 1rem;
  padding: 1rem;
  box-shadow: 0 3px 10px 3px rgba(0, 0, 0, 0.2);
}

.card__face-back {
  background-color: var(--light);
  transform: rotateY(-180deg);
}

.card__face-back .card__content {
  width: 100%;
  height: 100%;
  background-repeat: no-repeat;
  background-position: center center;
}

.card__face-front .card__content {
  background: url("../assets/images/icon_back.png") no-repeat center center;
  width: 100%;
  height: 100%;
}
</style>
