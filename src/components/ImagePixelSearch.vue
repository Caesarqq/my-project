<template>
<div>
    <canvas ref="canvas" @click="getPixelColor" @mousemove="getPixelColor"></canvas>
    <div v-if="pixelInfo">
    <p>Цвет: rgb({{ pixelInfo.color.r }}, {{ pixelInfo.color.g }}, {{ pixelInfo.color.b }})</p>
    <p>Координаты: x: {{ pixelInfo.x }}, y: {{ pixelInfo.y }}</p>
    <p>Размер изображения: ширина: {{ imageWidth }}px, высота: {{ imageHeight }}px</p>
    </div>
</div>
</template>
  
<script>
export default {
data() {
    return {
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
    const x = Math.floor((event.clientX - rect.left) * (canvas.width / rect.width));
    const y = Math.floor((event.clientY - rect.top) * (canvas.height / rect.height));
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
    loadImage(url) {
    const img = new Image();
    img.crossOrigin = 'Anonymous';
    img.onload = () => {
        this.drawImageToCanvas(img);
    };
    img.src = url;
    }
}
};
</script>
  
<style>
canvas {
width: 1500px;
height: 100vh;
}
</style>
  