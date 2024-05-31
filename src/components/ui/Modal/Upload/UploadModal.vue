<template>
  <div class="modal-wrapper">
    <div class="modal">
      <div class="modal__header header">
        <button class="header__button" @click="$emit('show')">X</button>
      </div>
      <label>
        URL:
        <input class="input_in" type="text" @change="savenewUrl" v-model="url" />
      </label>
      <label>
        Файл:
        <input
          class="input"
          type="file"
          ref="inputFile"
          @change="savenewFile"
        />
      </label>
      <button
        class="button"
        @click="saveResult(), $emit('show'), $emit('download')"
      >
        Загрузить
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "UploadModal",
  data() {
    return {
      Url: null,
      File: null,
      url: null,
      result: null,
    };
  },
  mounted() {
  },
  methods: {
    async savenewUrl() {
      fetch(this.url)
        .then((response) => response.blob())
        .then((blob) => {
          this.File = new Image();
          this.File.src = URL.createObjectURL(blob);
        });
    },
    savenewFile(e) {
      let img = e.target.files[0];
      this.File = new Image();
      this.File.src = URL.createObjectURL(img);
    },
    saveResult() {
      if (this.Url != null && this.File != null) {
        this.result = this.File;
      } else if (this.Url != null && this.File == null) {
        this.result = this.Url;
      } else if (this.Url == null && this.File != null) {
        this.result = this.File;
      }
      this.Url = null;
      this.url = null;
      this.$refs.inputFile.value = null;
      this.File = null;
    },
  },
};
</script>

<style scoped>
.modal-wrapper {
  height: 100vh;
  width: 100vw;
  background-color: rgba(27, 22, 36, 0.9);
  position: absolute;
  left: 0;
  top: 0;
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  background-color: #fff;
  align-items: center;
  height: 30vh;
  width: 30vw;
  padding: 10px;
  border-radius: 4px;
  border: 1px solid black;
}

.modal__header {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
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

label {
  display: flex;
  flex-direction: row;
  align-items: center;
  column-gap: 5px;
  margin-bottom: 10px;
}

.input {
  height: 20px;
  border-radius: 5px;
}

.input_in {
  width: 250px;
  border: 1px solid black;
  border-radius: 1px;
}
</style>
