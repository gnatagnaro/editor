<template>
  <div class="modal">
    <div class="modal__container header">
      <p class="header__text">Градационное преобразование</p>
      <button class="header__button" @click="$emit('show')">X</button>
    </div>
    <div class="modal__line">
      <div class="modal__element">
        <label class="modal__label">
          X1:
          <input
            type="number"
            v-model="x1"
            class="modal__input"
            @change="changeX1"
          />
        </label>
      </div>
      <div class="modal__element">
        <label class="modal__label">
          Y1:
          <input
            type="number"
            v-model="y1"
            class="modal__input"
            @change="changeY1"
          />
        </label>
      </div>
    </div>
    <div class="modal__line">
      <div class="modal__element">
        <label class="modal__label">
          X2:
          <input
            type="number"
            v-model="x2"
            class="modal__input"
            @change="changeX2"
          />
        </label>
      </div>
      <div class="modal__element">
        <label class="modal__label">
          Y2:
          <input
            type="number"
            v-model="y2"
            class="modal__input"
            @change="changeY2"
          />
        </label>
      </div>
    </div>
    <div class="modal__line">
      <label>
        <input type="checkbox" v-model="prewiev" @change="applyPrewiev" />
        Превью
      </label>
    </div>
    <div class="modal__line">
      <div class="modal__element">
        <button class="button" @click="correctImage(), $emit('apply')">Применить</button>
      </div>
      <div class="modal__element">
        <button class="button" @click="reset">Сброс</button>
      </div>
    </div>
    <div class="modal__line chart">
      <canvas id="chart" width="200" height="200" ref="chart"></canvas>
    </div>
  </div>
</template>

<script>
import Chart from "chart.js/auto";
export default {
  name: "CurvesModal",
  props: {
    dx: Number,
    dy: Number,
    nowW: Number,
    nowH: Number,
    ctxRef: CanvasRenderingContext2D,
    showCurvesModal: Boolean,
    startImage: Object,
  },
  data() {
    return {
      imgData: {},
      chartInstance: null,
      chartRef: null,
      prewiev: false,
      x1: 0,
      x2: 255,
      y1: 0,
      y2: 255,
    };
  },
  mounted() {
    this.chartRef = this.$refs.chart;
  },
  methods: {
    changeX1(){
      if (this.prewiev){
        this.correctImage();
        this.calculate();
      }
    },
    changeY1(){
      if (this.prewiev){
        this.correctImage();
        this.calculate();
      }
    },
    changeX2(){
      if (this.prewiev){
        this.correctImage();
        this.calculate();
      }
    },
    changeY2(){
      if (this.prewiev){
        this.correctImage();
        this.calculate();
      }
    },
    reset(){
      this.x1 = 0
      this.x2 = 255
      this.y1 = 0
      this.y2 = 255
      if (this.prewiev){
        this.correctImage();
        this.calculate();
      }
    },
    normalizeHistogram(histogram) {
      const max = Math.max(...histogram);
      return histogram.map((value) => (value / max) * 255);
    },
    applyPrewiev() {
      if (this.prewiev) {
        this.correctImage();
        this.calculate();
      } else {
        this.ctxRef.drawImage(
          this.startImage,
          this.dx,
          this.dy,
          this.nowW,
          this.nowH
        );
        this.calculate();
      }
    },
    correctImage() {
      this.ctxRef.drawImage(
          this.startImage,
          this.dx,
          this.dy,
          this.nowW,
          this.nowH
        );
      let lut = [];
      for (let i = 0; i < this.x1; i++) {
        lut[i] = this.y1;
      }
      for (let i = this.x1; i < this.x2; i++) {
        const slope = (this.y2 - this.y1) / (this.x2 - this.x1);
        let correctedValue = this.y1 + slope * (i - this.x1);
        correctedValue = Math.max(0, Math.min(255, correctedValue));
        lut[i] = correctedValue;
      }
      for (let i = this.x2; i < 256; i++) {
        lut[i] = this.y2;
      }

      const imageData = this.ctxRef.getImageData(
        this.dx,
        this.dy,
        this.nowW,
        this.nowH
      );
      const data = imageData.data;
      for (let i = 0; i < data.length; i += 4) {
        data[i] = lut[data[i]];
        data[i + 1] = lut[data[i + 1]];
        data[i + 2] = lut[data[i + 2]];
      }

      this.ctxRef.putImageData(imageData, this.dx, this.dy);
    },
    calculate() {
      let R = new Array(256).fill(0);
      let G = new Array(256).fill(0);
      let B = new Array(256).fill(0);
      let image = this.ctxRef.getImageData(
        this.dx,
        this.dy,
        this.nowW,
        this.nowH
      );
      let data = image.data;
      for (let i = 0; i < data.length; i += 4) {
        let red = data[i];
        let green = data[i + 1];
        let blue = data[i + 2];
        R[red]++;
        G[green]++;
        B[blue]++;
      }
      R = this.normalizeHistogram(R);
      G = this.normalizeHistogram(G);
      B = this.normalizeHistogram(B);
      this.imgData = {
        R: R,
        G: G,
        B: B,
      };
    },
    drawHistograms(redHistogram, greenHistogram, blueHistogram) {
      const ctx = this.chartRef?.getContext("2d");

      if (this.chartInstance) {
        this.chartInstance.destroy();
      }

      this.chartInstance = new Chart(ctx, {
        type: "scatter",
        data: {
          labels: Array.from({ length: 256 }, (_, i) => i),
          datasets: [
            {
              label: "Red",
              data: redHistogram,
              backgroundColor: "rgba(255, 0, 0, 0.8)",
            },
            {
              label: "Green",
              data: greenHistogram,
              backgroundColor: "rgba(0, 255, 0, 0.8)",
            },
            {
              label: "Blue",
              data: blueHistogram,
              backgroundColor: "rgba(0, 0, 255, 0.8)",
            },
            {
              label: "line",
              data: [
                { x: 0, y: this.y1 },
                { x: this.x1, y: this.y1 },
                { x: this.x2, y: this.y2 },
                { x: 255, y: this.y2 },
              ],
              borderColor: "rgba(0,0,0,1)",
              borderWidth: 1,
              fill: false,
              showLine: true,
            },
          ],
        },
        options: {
          animation: false,
          aspectRatio: 1,
          scales: {
            x: {
              type: "linear",
              position: "bottom",
              ticks: {
                stepSize: 7,
                color: 'gray',
              },
            },
            y: {
              beginAtZero: true,
              ticks: {
                stepSize: 7,
                color: 'gray',
              },
            }
          },
        },
      });
    },
  },
  watch: {
    showCurvesModal() {
      this.calculate();
    },
    imgData() {
      this.drawHistograms(this.imgData.R, this.imgData.G, this.imgData.B);
    },
    x1() {
      if (this.prewiev) {
        this.correctImage();
        this.calculate();
      }
    },
    x2() {
      if (this.prewiev) {
        this.correctImage();
        this.calculate();
      }
    },
    y1() {
      if (this.prewiev) {
        this.correctImage();
        this.calculate();
      }
    },
    y2() {
      if (this.prewiev) {
        this.correctImage();
        this.calculate();
      }
    },
  },
};
</script>

<style scoped>
.chart {
  padding: 5px;
  background-color: white;
  border-radius: 10px;
  border: #32373d solid 1px;
}

.modal {
  position: absolute;
  top: 110px;
  left: 60px;
  background-color: #fff;
  width: 25vw;
  padding: 15px;
  border-radius: 20px;
  display: flex;
  flex-direction: column;
  row-gap: 7px;
}

.modal__container {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
}

.modal__line {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
}

.modal__element {
  width: 100%;
  text-align: center;
}

.modal__label {
  display: flex;
  flex-direction: row;
  column-gap: 20px;
  align-items: center;
  justify-content: center;
}

.modal__input {
  width: 30%;
  border-radius: 6px;
  padding: 3px;
  border: 1px solid #ed6755;
}

.color-button {
  appearance: none;
  cursor: pointer;
  border: 1px solid black;
  border-radius: 0;
  width: 23px;
  height: 23px;
}

.color-button:checked {
  border: 2px solid black;
}

.button {
  background-color: #ed6755;
  border: none;
  border-radius: 5px;
  width: 110px;
  padding: 14px;
  font-size: 16px;
  color: white;
  box-shadow: 0px 6px 18px -5px rgba(237, 103, 85, 1);
  cursor: pointer;
}

.button:hover {
  background-color: #d55b49;
}

.header__text {
  font-size: 20px;
}

.header__button {
  width: 30px;
  font-size: 20px;
  color: #c0c5cb;
  background-color: transparent;
  border: none;
  cursor: pointer;
}
</style>
