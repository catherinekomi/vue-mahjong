<template>
  <div id="game-container">
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
        // Layer 1
        { rows: 1, cols: 12, offsetY: 0, startZ: 0 },
        { rows: 1, cols: 8, offsetY: 80, startZ: 0 },
        { rows: 1, cols: 10, offsetY: 160, startZ: 0 },
        { rows: 1, cols: 15, offsetY: 240, startZ: 0 },
        { rows: 1, cols: 12, offsetY: 320, startZ: 0 },
        { rows: 1, cols: 10, offsetY: 400, startZ: 0 },
        { rows: 1, cols: 8, offsetY: 480, startZ: 0 },
        { rows: 1, cols: 12, offsetY: 560, startZ: 0 },
        // Layer 2 (on top of 2nd row of Layer 1)
        { rows: 1, cols: 6, offsetY: 60, startZ: 1 },
        { rows: 1, cols: 6, offsetY: 140, startZ: 1 },
        { rows: 1, cols: 6, offsetY: 220, startZ: 1 },
        { rows: 1, cols: 6, offsetY: 300, startZ: 1 },
        { rows: 1, cols: 6, offsetY: 380, startZ: 1 },
        { rows: 1, cols: 6, offsetY: 460, startZ: 1 },
        // Layer 3
        { rows: 1, cols: 4, offsetY: 100, startZ: 2 },
        { rows: 1, cols: 4, offsetY: 180, startZ: 2 },
        { rows: 1, cols: 4, offsetY: 260, startZ: 2 },
        { rows: 1, cols: 4, offsetY: 340, startZ: 2 },

        // Layer 4
        { rows: 1, cols: 2, offsetY: 200, startZ: 3 },
        { rows: 1, cols: 2, offsetY: 280, startZ: 3 },

        // Layer 5
        { rows: 1, cols: 1, offsetY: 220, startZ: 4 },
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
      const boardWidth = 720;
      let tileIdCounter = 0;

      this.layers.forEach((layer) => {
        const { rows, cols, offsetY, startZ } = layer;
        for (let row = 0; row < rows; row++) {
          for (let col = 0; col < cols; col++) {
            const x = col * tileWidth + (boardWidth - cols * tileWidth) / 2;
            const y = offsetY;
            const z = startZ + row;
            const symbolIndex = Math.floor(Math.random() * this.tileTypes) + 1;

            this.tiles.push({
              id: `tile-${tileIdCounter++}`,
              symbol: symbolIndex,
              matched: false,
              position: { x, y, z },
            });
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
#game-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

#game-board {
  position: relative;
  width: 720px;
  height: 800px;
}
</style>
