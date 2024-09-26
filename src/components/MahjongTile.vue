<template>
  <div class="tile" :style="tileStyle" @click="handleClick">
    <img
      v-if="flipped || matched"
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
    flipped: Boolean,
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
      this.$emit('tile-clicked', this.symbol);
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
}
.tile img {
  max-width: 100%;
  max-height: 100%;
  object-fit: contain;
  border-radius: 10px;
}
</style>
