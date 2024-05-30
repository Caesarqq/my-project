<template>
<div>
  <v-tooltip bottom>
    <template v-slot:activator="{ on, attrs }">
      <v-btn
        v-bind="attrs"
        v-on="on"
        :color="currentTool === 'hand' ? 'red' : 'blue'"
        @click="selectTool('hand')"
      >
        Рука
      </v-btn>
    </template>
    <span>Перемещение изображение</span>
  </v-tooltip>

  <v-tooltip bottom>
    <template v-slot:activator="{ on, attrs }">
      <v-btn
        v-bind="attrs"
        v-on="on"
        :color="currentTool === 'pipette' ? 'red' : 'blue'"
        @click="selectTool('pipette')"
      >
        Пипетка
      </v-btn>
    </template>
    <span>Выбор цвета</span>
  </v-tooltip>
</div>
</template>

<script>
export default {
data() {
  return {
    currentTool: null
  };
},
methods: {
  selectTool(tool) {
    this.currentTool = tool;
  }
}
};
</script>

<style>
</style>