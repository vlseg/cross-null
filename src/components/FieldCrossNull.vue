<template>
	<div class="">
		<div class="field">
			<canvas id="canvas"></canvas>
			<div
				v-for="y in size"
				class="sell_vertical">
				<div
					:id="'sel'+y+'-'+x"
					v-for="x in size"
					class="sell"
					@click="drawCrossNull(y, x)"
				>&nbsp;
				</div>
			</div>
		</div>
	</div>
</template>

<script>
  export default {
    name: 'FieldCrossNull',
    props: {
      size: {
        type: Number
      }
    },
    data: () => ({
      visualCross: false,
      isToggleSign: false,
      mapSign: []
    }),
    mounted() {
      for (let i = 1; i < this.size + 1; i++){
        this.mapSign[i] = [];
        for (let j = 1; j < this.size + 1; j++){
          this.mapSign[i][j] = 0;
        }
      }

    },
    methods: {
      drawCrossNull(y, x, act) {
        const id = `sel${y}-${x}`;
        this.visualCross = true;
        this.isToggleSign = !this.isToggleSign;
        let symbol;
        if (this.isToggleSign) {
          document.getElementById(id).innerHTML = 'X';
          symbol = 1
        } else {
          document.getElementById(id).innerHTML = '0';
          symbol = 2
        }
        this.mapSign[y][x] = symbol;
        this.compareLineX(y, act);
        this.compareLineY(x, act);
        this.compareLineXY(y, x, act);
        this.compareLineYX(y, x, act);


      },

      line(x, y, x1, y1){
		    let line = document.getElementById("canvas"),
	      ctx = line.getContext('2d');
		    line.height = this.size * 20 + 20;
		    line.width  = this.size * 20 + 20;
		    ctx.beginPath();
		    ctx.moveTo(x, y);
		    ctx.lineTo(x1, y1);
		    ctx.stroke();
        line.style.zIndex = 0;
		  },

      compareLineXY(y, x) {
        // диагональная XY - \
        let x0 = x;
        let y0 = y;
        while (x !== 1 && y !== 1) {
          x0 = x0 - 1;
          y0 = y0 - 1;
          if (x0 === 1 || y0 === 1 || x0 === this.size || y0 === this.size) {
						break;
          }
        }
        let currentX = x0;
        let currentY = y0;
        let lastX = 0;
        let lastY = 0;
        let overlap = 1;
        const numSymbol = this.size < 5 ? 3 : 5;
        const delta = this.size < 5 ? 2 : 4;
        while (currentX < this.size) {
          if (currentX >= this.size || currentY >= this.size || currentX < 1 || currentY < 1) {
            break;
          }
          if (this.mapSign[currentY][currentX] === this.mapSign[currentY + 1][currentX + 1] && this.mapSign[currentY][currentX] !== 0) {
            overlap++;
            lastX = currentX + 1;
            lastY = currentY + 1;
          }
          if (overlap === numSymbol) {
            const startSellX = lastX - delta;
            const startSellY = lastY - delta;
            const idStart = `sel${startSellY}-${startSellX}`;
            const idEnd = `sel${lastY}-${lastX}`;
            const endX = document.getElementById(idEnd).offsetLeft + 15;
            const startX = document.getElementById(idStart).offsetLeft + 5;
            const endY = document.getElementById(idEnd).offsetTop + 15;
            const startY = document.getElementById(idStart).offsetTop + 5;
            this.line(startX, startY, endX, endY);
          }
          currentX++;
          currentY++;
        }
      },

      compareLineYX(y, x) {
        // диагональная YX - /
        let x0 = x;
        let y0 = y;
        while (x !== this.size && y !== 1) {
          x0 = x0 + 1;
          y0 = y0 - 1;
          if (x0 === 1 || y0 === 1 || x0 === this.size || y0 === this.size) {
	          break;
          }
        }
        let currentX = x0;
        let currentY = y0;
        let lastX = 0;
        let lastY = 0;
        let overlap = 1;
        const numSymbol = this.size < 5 ? 3 : 5;
        const delta = this.size < 5 ? 2 : 4;
        while (currentX <= this.size) {
          if (currentX > this.size || currentY >= this.size || currentX < 1 || currentY < 1) break;
          if (this.mapSign[currentY][currentX] === this.mapSign[currentY + 1][currentX - 1] && this.mapSign[currentY][currentX] !== 0) {
            overlap++;
            lastX = currentX - 1;
            lastY = currentY + 1;
          }
          if (overlap === numSymbol) {
            const startSellX = lastX + delta;
            const startSellY = lastY - delta;
            const idStart = `sel${startSellY}-${startSellX}`;
            const idEnd = `sel${lastY}-${lastX}`;
            const endX = document.getElementById(idEnd).offsetLeft + 5;
            const startX = document.getElementById(idStart).offsetLeft + 15;
            const endY = document.getElementById(idEnd).offsetTop + 15;
            const startY = document.getElementById(idStart).offsetTop + 5;
            this.line(startX, startY, endX, endY);
          }
          currentX--;
          currentY++;
        }
      },

      compareLineX(y) {
        // горизонтальная
        let currentX = 1;
        let lastX = 0;
        let overlap = 1;
        const numSymbol = this.size < 5 ? 3 : 5;
        const delta = this.size < 5 ? 2 : 4;
        while (currentX < this.size) {
          if (this.mapSign[y][currentX] === this.mapSign[y][currentX + 1] && this.mapSign[y][currentX] !== 0) {
            overlap++;
            lastX = currentX + 1;
          }
          if (overlap === numSymbol) {
            const startSell = lastX - delta;
            const idStart = `sel${y}-${startSell}`;
            const idEnd = `sel${y}-${lastX}`;
            const endX = document.getElementById(idEnd).offsetLeft + 15;
            const startX = document.getElementById(idStart).offsetLeft + 5;
            const constY = document.getElementById(idEnd).offsetTop + 10;
            this.line(startX, constY, endX, constY);
          }
          currentX++
        }
      },

      compareLineY(x) {
        // вертикальная
        let currentY = 1;
        let lastY = 0;
        let overlap = 1;
        const numSymbol = this.size < 5 ? 3 : 5;
        const delta = this.size < 5 ? 2 : 4;
        while (currentY < this.size) {
          if (this.mapSign[currentY][x] === this.mapSign[currentY + 1][x] && this.mapSign[currentY][x] !== 0) {
            overlap++;
            lastY = currentY + 1;
          }
          if (overlap === numSymbol) {
            const startSell = lastY - delta;
            const idStart = `sel${startSell}-${x}`;
            const idEnd = `sel${lastY}-${x}`;
            const endY = document.getElementById(idEnd).offsetTop + 15;
            const startY = document.getElementById(idStart).offsetTop + 5;
            const constX = document.getElementById(idEnd).offsetLeft + 10;
            this.line(constX, startY, constX, endY);
          }
          currentY++
        }
      }
    }
  }
</script>

<style scoped>
	.sell {
		display: inline-block;
		border: 1px solid blue;
		width: 20px;
		height: 19px;
		padding-top: 1px;
		cursor: pointer;
		text-align: center;
	}
	.field {
		position: relative;
	}
	#canvas {
		position: absolute;
		z-index: -1;
	}
</style>
