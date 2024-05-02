<template>
  <v-container>
    <canvas ref="canvas" @click="getPixelColor" @mousemove="getPixelColor"></canvas>
    <div v-if="pixelInfo">
      <p>Цвет: rgb({{ pixelInfo.color.r }}, {{ pixelInfo.color.g }}, {{ pixelInfo.color.b }})</p>
      <p>Координаты: x: {{ pixelInfo.x }}, y: {{ pixelInfo.y }}</p>
      <p>Размер изображения: ширина: {{ imageWidth }}px, высота: {{ imageHeight }}px</p>
    </div>
    <div class="d-flex flex-column">
      <div>
        <div class="text-caption">
          Масштабирование
        </div>
        <v-slider
          v-model="slider2"
          thumb-label="always"
        ></v-slider>
      </div>
    </div>
    <v-form v-if="type === 'local'">
      <v-file-input label="Выберите файл" @change="uploadLocal"></v-file-input>
    </v-form>
    <v-form v-if="type === 'url'">
      <v-text-field label="Введите URL изображения" v-model="imageUrl"></v-text-field>
      <v-btn @click="uploadFromUrl">Загрузить</v-btn>
    </v-form>
  </v-container>
</template>

<script>
export default {
  props: {
    type: String
  },
  data() {
    return {
      slider2: 100,
      imageUrl: "",
      pixelInfo: null,
      imageWidth: 0,
      imageHeight: 0
    };
  },
  methods: {
    drawImageToCanvas(image) {
      const canvas = this.$refs.canvas;
      const ctx = canvas.getContext('2d');
      this.imageWidth = image.width;
      this.imageHeight = image.height;
      canvas.width = image.width;
      canvas.height = image.height;
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      ctx.drawImage(image, 0, 0);
    },
    getPixelColor(event) {
      const canvas = this.$refs.canvas;
      const ctx = canvas.getContext('2d');
      const rect = canvas.getBoundingClientRect();
      const x = event.clientX - rect.left;
      const y = event.clientY - rect.top;
      const pixel = ctx.getImageData(x, y, 1, 1).data;
      this.pixelInfo = {
        color: {
          r: pixel[0],
          g: pixel[1],
          b: pixel[2]
        },
        x: x,
        y: y
      };
    },
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
      img.onload = () => {
        this.drawImageToCanvas(img);
      };
      img.src = this.imageUrl;
    }
  }
}
</script>

<style>
canvas {
  width: 100%;
  height: 100vh;
}
</style>
