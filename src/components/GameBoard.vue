<template>
  <div id="game-board">
    <MahjongTile
      v-for="tile in tiles"
      :key="tile.id"
      :symbol="tile.symbol"
      :matched="tile.matched"
      :flipped="tile.isFlipped"
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
                isFlipped: false,
                matched: false,
                isFree: false,
                position: { x, y, z },
              });
            }
          }
        }
      });

      this.checkFreeTiles();
    },
    generateTilePositions() {
      const positions = [];
      const layers = [
        { rows: 4, cols: 14, startX: 3, startY: 2 },
        { rows: 2, cols: 10, startX: 4, startY: 3 },
      ];

      const tileWidth = 60;
      const tileHeight = 80;

      const offsetX = 100;
      const offsetY = 20;

      layers.forEach((layer, index) => {
        const layerOffsetX = layer.startX * tileWidth;
        const layerOffsetY = layer.startY * tileHeight;

        for (let row = 0; row < layer.rows; row++) {
          for (let col = 0; col < layer.cols; col++) {
            positions.push({
              x: offsetX + layerOffsetX + col * tileWidth,
              y: offsetY + layerOffsetY + row * tileHeight,
              z: index,
            });
          }
        }
      });

      return positions;
    },
    checkFreeTiles() {
      this.tiles.forEach((tile) => {
        const aboveTile = this.tiles.find(
          (t) =>
            t.position.x === tile.position.x &&
            t.position.y === tile.position.y &&
            t.position.z === tile.position.z + 1
        );
        tile.isFree = !aboveTile;
      });
    },
    handleTileClick(tile) {
      this.$emit('tile-clicked', tile.symbol);
    },
    checkForMatch() {
      if (this.firstTile.symbol === this.secondTile.symbol) {
        this.firstTile.matched = true;
        this.secondTile.matched = true;
      } else {
        setTimeout(() => {
          this.firstTile.isFlipped = true;
          this.secondTile.isFlipped = true;
        }, 1000);
      }
      this.resetSelectedTiles();
    },
    resetSelectedTiles() {
      this.firstTile = null;
      this.secondTile = null;
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
