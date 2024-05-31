<template>
  <div class="modal">
    <div class="modal__container header">
      <p class="header__text">Фильтрация</p>
      <button class="header__button" @click="$emit('show')">X</button>
    </div>
    <div class="modal__line">
      <div class="modal__element matrix">
        <div class="matrix__line">
          <input
            class="matrix__input"
            type="number"
            @change="changeX1"
            v-model="x1"
          />
          <input
            class="matrix__input"
            type="number"
            @change="changeX2"
            v-model="x2"
          />
          <input
            class="matrix__input"
            type="number"
            @change="changeX3"
            v-model="x3"
          />
        </div>
        <div class="matrix__line">
          <input
            class="matrix__input"
            type="number"
            @change="changeX4"
            v-model="x4"
          />
          <input
            class="matrix__input"
            type="number"
            @change="changeX5"
            v-model="x5"
          />
          <input
            class="matrix__input"
            type="number"
            @change="changeX6"
            v-model="x6"
          />
        </div>
        <div class="matrix__line">
          <input
            class="matrix__input"
            type="number"
            @change="changeX7"
            v-model="x7"
          />
          <input
            class="matrix__input"
            type="number"
            @change="changeX8"
            v-model="x8"
          />
          <input
            class="matrix__input"
            type="number"
            @change="changeX9"
            v-model="x9"
          />
        </div>
      </div>
      <div class="modal__element type">
        <label class="modal__label">
          <input type="radio" name="type" value="same" v-model="type"  />
          Тождественное
        </label>
        <label class="modal__label">
          <input type="radio" name="type" value="sharp" v-model="type"  />
          Резкость
        </label>
        <label class="modal__label">
          <input type="radio" name="type" value="gaus" v-model="type"  />
          Гаусс
        </label>
        <label class="modal__label">
          <input type="radio" name="type" value="rect" v-model="type"  />
          Прямоугольное
        </label>
        <label class="modal__label">
          <input type="radio" name="type" value="sobel" v-model="type"  />
          Собель
        </label>
      </div>
    </div>
    <div class="modal__line">
      <label>
        <input type="checkbox" v-model="preview" @change="applyPreview" />
        Превью
      </label>
    </div>
    <div class="modal__line">
      <div class="modal__element">
        <button class="modal__button" @click="applyFilter">Применить</button>
      </div>
      <div class="modal__element">
        <button class="modal__button" @click="reset">Сбросить</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "FilteringModal",
  props: {
    dx: Number,
    dy: Number,
    nowW: Number,
    nowH: Number,
    ctxRef: CanvasRenderingContext2D,
    startImage: Object,
  },
  data() {
    return {
      preview: false,
      type: "same",
      x1: 0,
      x2: 0,
      x3: 0,
      x4: 0,
      x5: 1,
      x6: 0,
      x7: 0,
      x8: 0,
      x9: 0,
      kernel: [
        [0, 0, 0],
        [0, 1, 0],
        [0, 0, 0],
      ],
      sobelX: [
        [-1, 0, 1],
        [-2, 0, 2],
        [-1, 0, 1],
      ],
      sobelY: [
        [-1, -2, -1],
        [0, 0, 0],
        [1, 2, 1],
      ],
    };
  },
  methods: {
    changeX1() {
      this.kernel[0][0] = Number(this.x1);
      if (this.preview) {
        this.applyFilter();
      }
    },
    changeX2() {
      this.kernel[0][1] = Number(this.x2);
      if (this.preview) {
        this.applyFilter();
      }
    },
    changeX3() {
      this.kernel[0][2] = Number(this.x3);
      if (this.preview) {
        this.applyFilter();
      }
    },
    changeX4() {
      this.kernel[1][0] = Number(this.x4);
      if (this.preview) {
        this.applyFilter();
      }
    },
    changeX5() {
      this.kernel[1][1] = Number(this.x5);
      if (this.preview) {
        this.applyFilter();
      }
    },
    changeX6() {
      this.kernel[1][2] = Number(this.x6);
      if (this.preview) {
        this.applyFilter();
      }
    },
    changeX7() {
      this.kernel[2][0] = Number(this.x7);
      if (this.preview) {
        this.applyFilter();
      }
    },
    changeX8() {
      this.kernel[2][1] = Number(this.x8);
      if (this.preview) {
        this.applyFilter();
      }
    },
    changeX9() {
      this.kernel[2][2] = Number(this.x9);
      if (this.preview) {
        this.applyFilter();
      }
    },
    reset() {
      this.type = "same";
      if (this.preview) {
        this.ctxRef.drawImage(
          this.startImage,
          this.dx,
          this.dy,
          this.nowW,
          this.nowH
        );
      }
    },
    applyPreview() {
      if (this.preview) {
        this.applyFilter();
      } else {
        this.ctxRef.drawImage(
          this.startImage,
          this.dx,
          this.dy,
          this.nowW,
          this.nowH
        );
      }
    },
    applyFilter() {
      if (!this.preview && this.type !== "sobel") {
        this.ctxRef.drawImage(
          this.startImage,
          this.dx,
          this.dy,
          this.nowW,
          this.nowH
        );
      }

      const imageData = this.ctxRef.getImageData(
        this.dx,
        this.dy,
        this.nowW,
        this.nowH
      );
      const newData = new Uint8ClampedArray(imageData.data.length);

      const paddedData = this.padImageData(
        imageData.data,
        imageData.width,
        imageData.height
      );

      if (this.type === "sobel") {
        const gradientX = this.applyKernel(
          paddedData,
          imageData.width,
          imageData.height,
          this.sobelX
        );
        const gradientY = this.applyKernel(
          paddedData,
          imageData.width,
          imageData.height,
          this.sobelY
        );
        for (let i = 0; i < newData.length; i += 4) {
          const magnitude = Math.sqrt(
            gradientX[i] ** 2 + gradientY[i] ** 2
          );
          newData[i] = magnitude;
          newData[i + 1] = magnitude;
          newData[i + 2] = magnitude;
          newData[i + 3] = 255; // Alpha channel
        }
      } else {
        for (let y = 0; y < imageData.height; y++) {
          for (let x = 0; x < imageData.width; x++) {
            for (let c = 0; c < 4; c++) {
              const outputIndex = (y * imageData.width + x) * 4 + c;
              let sum = 0;
              let kernelSum = 0;
              for (let ky = 0; ky < 3; ky++) {
                for (let kx = 0; kx < 3; kx++) {
                  const inputIndex =
                    ((y + ky) * (imageData.width + 2) + (x + kx)) * 4 + c;
                  sum += paddedData[inputIndex] * this.kernel[ky][kx];
                  kernelSum += this.kernel[ky][kx];
                }
              }
              newData[outputIndex] = sum / kernelSum;
            }
          }
        }
      }

      imageData.data.set(newData);
      this.ctxRef.putImageData(imageData, this.dx, this.dy);
    },
    applyKernel(paddedData, width, height, kernel) {
      const result = new Uint8ClampedArray(paddedData.length);
      for (let y = 0; y < height; y++) {
        for (let x = 0; x < width; x++) {
          for (let c = 0; c < 4; c++) {
            let sum = 0;
            for (let ky = 0; ky < 3; ky++) {
              for (let kx = 0; kx < 3; kx++) {
                const inputIndex =
                  ((y + ky) * (width + 2) + (x + kx)) * 4 + c;
                sum += paddedData[inputIndex] * kernel[ky][kx];
              }
            }
            result[(y * width + x) * 4 + c] = sum;
          }
        }
      }
      return result;
    },
    padImageData(data, width, height) {
      const paddedWidth = width + 2;
      const paddedHeight = height + 2;
      const paddedData = new Uint8ClampedArray(paddedWidth * paddedHeight * 4);

      for (let y = 0; y < height; y++) {
        for (let x = 0; x < width; x++) {
          const inputIndex = (y * width + x) * 4;
          const outputIndex = ((y + 1) * paddedWidth + x + 1) * 4;
          paddedData.set(
            data.subarray(inputIndex, inputIndex + 4),
            outputIndex
          );
        }
      }

      for (let y = 0; y < paddedHeight; y++) {
        for (let x = 0; x < paddedWidth; x++) {
          const outputIndex = (y * paddedWidth + x) * 4;
          if (
            x === 0 ||
            x === paddedWidth - 1 ||
            y === 0 ||
            y === paddedHeight - 1
          ) {
            const nearestX = Math.max(1, Math.min(x, paddedWidth - 2));
            const nearestY = Math.max(1, Math.min(y, paddedHeight - 2));
            const nearestIndex = (nearestY * paddedWidth + nearestX) * 4;
            paddedData.set(
              paddedData.subarray(nearestIndex, nearestIndex + 4),
              outputIndex
            );
          }
        }
      }
      return paddedData;
    },
  },
  watch: {
    type: function (value) {
      if (value === "same") {
        this.kernel = [
          [0, 0, 0],
          [0, 1, 0],
          [0, 0, 0],
        ];
      }
      if (value === "sharp") {
        this.kernel = [
          [-1, -1, -1],
          [-1, 9, -1],
          [-1, -1, -1],
        ];
      }
      if (value === "gaus") {
        this.kernel = [
          [1, 2, 1],
          [2, 4, 2],
          [1, 2, 1],
        ];
      }
      if (value === "rect") {
        this.kernel = [
          [1, 1, 1],
          [1, 1, 1],
          [1, 1, 1],
        ];
      }
      if (value === "sobel") {
        this.kernel = [
          [1, 0, -1],
          [1, 0, -1],
          [1, 0, -1],
        ];
      }
      if (this.preview) {
        this.applyFilter();
      }
    },
    kernel: function (value) {
      this.x1 = value[0][0];
      this.x2 = value[0][1];
      this.x3 = value[0][2];
      this.x4 = value[1][0];
      this.x5 = value[1][1];
      this.x6 = value[1][2];
      this.x7 = value[2][0];
      this.x8 = value[2][1];
      this.x9 = value[2][2];
    },
  },
};
</script>
<style scoped>
.modal {
  position: absolute;
  bottom: -20px;
  left: 12px;
  background-color: #fff;
  width: 25vw;
  padding: 15px;
  border-radius: 20px;
  display: flex;
  flex-direction: column;
  row-gap: 20px;
}

.modal__element {
  display: flex;
  text-align: left;
  align-items: left;
  justify-content: flex-start;
  flex-direction: column;
}

.modal__container {
  display: flex;
  flex-direction: row;
  /* align-items: center; */
  justify-content: space-between;
}

.modal__line {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
}

.modal__element {
  width: 50%;
  text-align: center;
}

.modal__button {
  background-color: #6ba6bf;
  border: none;
  border-radius: 5px;
  width: 110px;
  padding: 14px;
  font-size: 16px;
  color: white;
  box-shadow: 0px 6px 18px -5px rgb(38, 25, 23);
  cursor: pointer;
}

.modal__label {
  display: flex;
  flex-direction: row;
  column-gap: 20px;
  align-items: center;
}

.modal__input {
  width: 30%;
  border-radius: 6px;
  padding: 3px;
  border: 1px solid #6ba6bf;
}

.type {
  display: flex;
  flex-direction: column;
  /* align-items: center; */
  margin-left: 20px;
}

.matrix {
  display: flex;
  flex-direction: column;
  row-gap: 5px;
}

.matrix__line {
  display: flex;
  flex-direction: row;
  column-gap: 5px;
}

.matrix__input {
  width: 45px;
  border: 1px solid #6ba6bf;
  padding: 3px;
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