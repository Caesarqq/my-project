<template>
  <div class="canvasmain">
    <image-pixel-search ref="pixelSearch"></image-pixel-search>
    <v-slider
      v-model="slider"
      :max="max"
      :min="min"
      class="align-center"
      hide-details
    >
      <template v-slot:append>
        <v-text-field
          v-model="sliderPercent"
          density="compact"
          style="width: 40px"
          type="text"
          hide-details
          single-line
        ></v-text-field>
      </template>
    </v-slider>
    <v-form v-if="type === 'local'">
      <v-file-input label="Выберите файл" @change="uploadLocal"></v-file-input>
    </v-form>
    <v-form v-if="type === 'url'">
      <v-text-field label="Введите URL изображения" v-model="imageUrl"></v-text-field>
      <v-btn @click="uploadFromUrl">Загрузить</v-btn>
    </v-form>
  </div>
</template>

<script>
import ImagePixelSearch from './ImagePixelSearch.vue';

export default {
  components: {
    ImagePixelSearch
  },
  props: {
    type: String
  },
  data() {
    return {
      min: 12,
      max: 300,
      slider: 100,
      imageUrl: ""
    };
  },
  computed: {
    sliderPercent() {
      return `${this.slider}%`;
    }
  },
  methods: {
    uploadLocal(file) {
      const reader = new FileReader();
      reader.onload = (e) => {
        const img = new Image();
        img.onload = () => {
          this.drawImageToCanvas(img);
        };
        img.src = e.target.result;
      };
      reader.readAsDataURL(file);
    },
    uploadFromUrl() {
      const img = new Image();
      img.crossOrigin = 'Anonymous';
      img.onload = () => {
        this.drawImageToCanvas(img);
      };
      img.src = this.imageUrl;
    },
    drawImageToCanvas(image) {
      const canvas = this.$refs.pixelSearch.$refs.canvas;
      const ctx = canvas.getContext('2d');
      this.$refs.pixelSearch.imageWidth = image.width;
      this.$refs.pixelSearch.imageHeight = image.height;
      canvas.width = image.width;
      canvas.height = image.height;
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      ctx.drawImage(image, 0, 0);
    }
  }
}
</script>

<style>
/* body{
  overflow: hidden;
} */
canvas {
  width: 1500px;
  height: 100vh;
}
.row{
  margin: 0px;
}

/* .canvasmain {
  display: flex;
  justify-content: center; 
  width: 100%;
} */
</style>
