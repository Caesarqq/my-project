<template>
  <v-app>
    <v-layout row justify-center>
      <v-dialog v-model="dialog" persistent max-width="500">
        <template v-slot:activator="{ on }">
          <v-btn color="primary" dark v-on="on" class="mt-5">Загрузить изображение</v-btn>
        </template>
        <v-card>
          <v-card-title class="headline">Хотите загрузить изображение?</v-card-title>
          <v-card-text>Выбирете способ загрузки изображения:</v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="green darken-1" flat @click="setUploadType('local')">Локально</v-btn>
            <v-btn color="red darken-1" flat @click="setUploadType('url')">URL</v-btn>
            <v-spacer></v-spacer>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </v-layout>
    <image-uploader v-if="uploadType" :type="uploadType"></image-uploader>
  </v-app>
</template>

<script>
import ImageUploader from './components/ImageUploader.vue';

export default {
  components: {
    ImageUploader
  },
  data () {
    return {
      dialog: false,
      uploadType: null
    }
  },
  methods: {
    setUploadType(type) {
      this.uploadType = type;
      this.dialog = false;
    }
  }
}
</script>