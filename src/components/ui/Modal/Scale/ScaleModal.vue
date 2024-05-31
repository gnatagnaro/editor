<template>
  <dialog class="modal">
    <div class="modal__header">
      <button class="header__button" @click="closeModal">X</button>
    </div>
    <div class="modal__body">
      <p class="header__text">Масштабирование</p>
      <label>
        Способ изменения размера
        <select v-model="type" @change="chooseType" class="custom-select">
          <option value="pixels">В пикселях</option>
          <option value="persentage">В процентах</option>
        </select>
      </label>
      <div class="modal__container">
        <label>
          Ширина:
          <input
            class="input"
            type="number"
            min="1"
            v-model="width"
            @change="updateWidth"
          />
        </label>
        <label>
          Высота:
          <input
            class="input"
            type="number"
            min="1"
            v-model="height"
            @change="updateHeight"
          />
        </label>
      </div>
      <div class="modal__container">
        <p>Разрешение до: {{ resNow }} MP</p>
        <p>Разрешение после: {{ resAfter }} MP</p>
      </div>
      <label>
        Сохранять заданные пропорции:
        <input type="checkbox" v-model="proprtions" />
      </label>
      <label>
        Интерполяция:
        <select v-model="interpol" class="custom-select">
          <option value="nearest">Ближайшие соседи</option>
        </select>
      </label>
    </div>
    <div class="modal__footer">
      <button class="button" @click="resizeAndClose">Отобразить</button>
    </div>
  </dialog>
</template>

<script>
export default {
  name: "ScaleModal",
  props: {
    nowW: Number,
    nowH: Number,
  },
  data() {
    return {
      type: "pixels",
      width: this.nowW,
      height: this.nowH,
      outputH: null,
      outputW: null,
      proprtions: false,
      interpol: "nearest",
    };
  },
  watch: {
    nowW(value) {
      this.width = value;
    },
    nowH(value) {
      this.height = value;
    },
  },
  computed: {
    resNow() {
      return Math.round((this.nowH * this.nowW) / 10000) / 100;
    },
    resAfter() {
      return Math.round((this.outputH * this.outputW) / 10000) / 100;
    },
  },
  methods: {
    chooseType() {
      if (this.type === "persentage") {
        this.width = (this.width / this.nowW) * 100;
        this.height = (this.height / this.nowH) * 100;
      } else {
        this.width = (this.width * this.nowW) / 100;
        this.height = (this.height * this.nowH) / 100;
      }
    },
    updateHeight() {
      if (this.proprtions) {
        let coef = this.nowW / this.nowH;
        this.width = Math.round(this.height * coef);
      }
      if (this.type === "persentage") {
        this.outputH = Math.round((this.height * this.nowH) / 100);
        this.outputW = Math.round((this.width * this.nowW) / 100);
      } else {
        this.outputH = this.height;
        this.outputW = this.width;
      }
    },
    updateWidth() {
      if (this.proprtions) {
        let coef = this.nowW / this.nowH;
        this.height = Math.round(this.width * coef);
      }
      if (this.type === "persentage") {
        this.outputH = Math.round((this.height * this.nowH) / 100);
        this.outputW = Math.round((this.width * this.nowW) / 100);
      } else {
        this.outputH = this.height;
        this.outputW = this.width;
      }
    },
    closeModal() {
      this.$emit('show');
    },
    resizeAndClose() {
      this.$emit('resize');
      this.closeModal();
    },
  },
};
</script>

<style scoped>

label {
  display: flex;
  flex-direction: row;
  align-items: center;
  column-gap: 5px;
  margin-bottom: 10px;
}

.modal {
  display: flex;
  top: 30px;
  left: 200px;
  flex-direction: column;
  align-items: center;
  background-color: #fff;
  color: #000;
  text-align: center;
  border-radius: 20px;
  padding: 30px 30px 70px;
  border: 1px solid #0d1b2a;
  box-shadow: 0px 6px 18px -5px rgba(0, 0, 0, 0.1);
}
.modal__header,
.modal__footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
}
.modal__body {
  display: flex;
  flex-direction: column;
  gap: 15px;
  width: 100%;
}
.header__text {
  font-size: 20px;
  font-weight: bold;
}
.header__button {
  width: 30px;
  font-size: 20px;
  color: #c0c5cb;
  background-color: transparent;
  border: none;
  cursor: pointer;
}
.button {
  background-color: #ed6755;
  border: none;
  border-radius: 5px;
  width: 200px;
  padding: 14px;
  font-size: 16px;
  color: white;
  box-shadow: 0px 6px 18px -5px rgba(237, 103, 85, 1);
  cursor: pointer;
}
.button:hover {
  background-color: #d55b49;
}
.input {
  width: 100px;
  padding: 5px;
  border: 1px solid #0d1b2a;
  border-radius: 5px;
}
.custom-select {
  appearance: none;
  -webkit-appearance: none;
  -moz-appearance: none;
  background-color: #fff;
  border: 1px solid #0d1b2a;
  border-radius: 5px;
  padding: 5px;
  padding-right: 30px; /* space for the dropdown arrow */
  background-image: url('data:image/svg+xml;charset=US-ASCII,<svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="black"><path d="M4.293 6.293l4 4 4-4 .707.707L8 11.707l-4.707-4.707z"/></svg>');
  background-repeat: no-repeat;
  background-position: right 10px top 50%;
  background-size: 16px 16px;
  cursor: pointer;
}
.custom-select option {
  padding: 10px;
}
</style>
