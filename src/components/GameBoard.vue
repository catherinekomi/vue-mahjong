<template>
  <div id="game-board">
    <MahjongTile
      v-for="tile in tiles"
      :key="tile.id"
      :symbol="tile.symbol"
      :matched="tile.matched"
      :position="tile.position"
      @tile-clicked="handleTileClick(tile)"
    />
  </div>
</template>

<script>
import MahjongTile from './MahjongTile.vue';

export default {
  components: {
    MahjongTile,
  },
  data() {
    return {
      tiles: [],
      tileTypes: 36,
      layers: [
        { rows: 4, cols: 12, offsetX: 0, offsetY: 0, startZ: 0 },
        { rows: 3, cols: 10, offsetX: 1, offsetY: 1, startZ: 1 },
        { rows: 2, cols: 8, offsetX: 2, offsetY: 2, startZ: 2 },
        { rows: 1, cols: 6, offsetX: 3, offsetY: 3, startZ: 3 },
      ],
      firstTile: null,
      secondTile: null,
    };
  },
  created() {
    this.initializeTiles();
  },
  methods: {
    initializeTiles() {
      const tileWidth = 60;
      const tileHeight = 80;
      const boardWidth = 800;
      let tileIdCounter = 0;

      this.layers.forEach((layer) => {
        const { rows, cols, offsetX, offsetY, startZ } = layer;
        for (let row = 0; row < rows; row++) {
          for (let col = 0; col < cols; col++) {
            const x =
              (offsetX + col) * tileWidth + (boardWidth - cols * tileWidth) / 2;
            const y = (offsetY + row) * tileHeight;
            const z = startZ + row;

            const symbolIndex = Math.floor(Math.random() * this.tileTypes) + 1;

            for (let i = 0; i < 4; i++) {
              this.tiles.push({
                id: `tile-${tileIdCounter++}`,
                symbol: symbolIndex,
                matched: false,
                position: { x, y, z },
              });
            }
          }
        }
      });
    },
    handleTileClick(tile) {
      if (tile.matched) return;
      if (!this.firstTile) {
        this.firstTile = tile;
      } else if (!this.secondTile) {
        this.secondTile = tile;
        this.checkForMatch();
      }
    },
    checkForMatch() {
      if (this.firstTile.symbol === this.secondTile.symbol) {
        this.firstTile.matched = true;
        this.secondTile.matched = true;
      }
      setTimeout(() => {
        this.firstTile = null;
        this.secondTile = null;
      }, 1000);
    },
  },
};
</script>

<style scoped>
#game-board {
  position: relative;
  width: 100%;
  height: 600px;
  background-color: #142800;
  overflow: hidden;
}
</style>
