<template>
  <div class="content">
    <p class="font">x: {{ x }}px, y: {{ y }}px</p>
    <template v-if="showData">
      <div class="params">
        <p class="font">Ширина: {{ width }}px</p>
        <p class="font">Высота: {{ height }}px</p>
      </div>
      <select :disabled="state!=''" v-model="scale" @change="$emit('scale')">
        <option value="12">12%</option>
        <option value="25">25%</option>
        <option value="50">50%</option>
        <option value="100">100%</option>
        <option value="150">150%</option>
        <option value="200">200%</option>
        <option value="300">300%</option>
      </select>
      <div v-if="resl != null" class="color">
          <div class="color__img" :style="string"></div>
          <div class="color__inf">
            <p class="color__font">{{ color }}</p>
            <p class="color__font">Hex: {{ HEX }}</p>
          </div>
        </div>
    </template>
  </div>
</template>

  <script>
export default {
  name: "AsidePanel",
  props: {
    x: Number,
    y: Number,
    height: Number,
    width: Number,
    showData: Boolean,
    resl: Array,
    state: String
  },
  data() {
    return {
      picker: null,
      scale: 100,
    }
  },
  mounted() {
  },
  methods: {
  },
  computed: {
    string() {
      return (
        "background-color: rgb(" +
        this.resl[0] +
        "," +
        this.resl[1] +
        "," +
        this.resl[2] +
        ");"
      );
    },
    color() {
      return (
        "rgb(" + this.resl[0] + ", " + this.resl[1] + ", " + this.resl[2] + ")"
      );
    },
    HEX() {
      return (
        "#" +
        [this.resl[0], this.resl[1], this.resl[2]]
          .map((x) => {
            const hex = x.toString(16);
            return hex.length === 1 ? "0" + hex : hex;
          })
          .join("")
      );
    },
  }
};
</script>

<style scoped>
.content {
  width: 2000px;
  height: 683px;
  border-radius: 10px;
  background-color: gray;
  display: flex;
  align-items: center;
  flex-direction: column;
  row-gap: 20px;
  padding: 20px;
  margin-top: 160px;
  margin-right: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.color {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  column-gap: 10px;
  margin-top: 10px;
  font-size: 14px;
}

.color__img {
  height: 35px;
  width: 35px;
  border-radius: 50%;
  border: 1px solid #e0e1dd;
}

.color__font {
  color: #e0e1dd;
}

.color__inf {
  display: flex;
  flex-direction: column;
  row-gap: 10px;
}

.font {
  color: #e0e1dd;
}

.font_center {
  align-self: center;
}

.params {
  padding-left: 15px;
  display: flex;
  flex-direction: column;
  row-gap: 7px;
}

.select-container {
  width: 100%;
}

select {
  width: 100%;
  padding: 10px;
  border-radius: 5px;
  border: 1px solid #ccc;
  background-color: gray;
  color: #e0e1dd;
  font-size: 16px;
  appearance: none;
  background-repeat: no-repeat;
  background-position: right 10px center;
  background-size: 12px;
}

select:focus {
  outline: none;
  border-color: gray;
}
</style>
