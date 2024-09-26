<template>
  <div
    class="tile"
    :style="tileStyle"
    @click="handleClick"
    :class="{ hidden: matched }"
  >
    <img
      v-if="!matched"
      :src="tileImage"
      alt="Mahjong Tile"
      @error="imageError"
    />
  </div>
</template>

<script>
export default {
  props: {
    symbol: Number,
    matched: Boolean,
    position: Object,
  },
  computed: {
    tileStyle() {
      return {
        left: `${this.position.x}px`,
        top: `${this.position.y}px`,
        zIndex: this.position.z || 1,
        position: 'absolute',
      };
    },
    tileImage() {
      return require(`@/assets/mahjong${this.symbol}.png`);
    },
  },
  methods: {
    handleClick() {
      if (!this.matched) {
        this.$emit('tile-clicked', this.symbol);
      }
    },
    imageError() {
      console.error(`Image not found for tile: mahjong${this.symbol}.png`);
    },
  },
};
</script>

<style scoped>
.tile {
  width: 60px;
  height: 80px;
  background-color: white;
  border: 2px solid #333;
  border-radius: 10px;
  position: absolute;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  transition: opacity 0.5s ease;
  box-shadow: 5px 5px 0px rgba(255, 165, 0, 0.6),
    5px 5px 10px rgba(0, 0, 0, 0.8);
  border-right: 6px solid #000;
  border-bottom: 6px solid #000;
}

.tile.hidden {
  opacity: 0;
  pointer-events: none;
}

.tile img {
  max-width: 100%;
  max-height: 100%;
  object-fit: contain;
  border-radius: 10px;
}
</style>
