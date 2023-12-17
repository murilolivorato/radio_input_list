<template>
  <div class="container">
    <h3>The Selected ID is - {{ form.car }}</h3>
    <radio-input-list :dividedby="3"
                      :list="list"
                      :valueSelect="form.car"
                      v-model:value="form.car"
                      name="car" ></radio-input-list>
  </div>
</template>
<script setup>
import RadioInputList from '@/components/RadioInputList.vue'
import { reactive, ref } from 'vue'
const list = ref([{ id: '1', title: 'Jaguar' }, { id: '2', title: 'Audi' }, { id: '3', title: 'Porsche' }, { id: '4', title: 'Lamborghini'}, { id: '5', title: 'Mercedes-Benz'}])
const form = reactive({ car: 1})
</script>
<style scoped>
h3 {
  padding-bottom:45px;
  font-size: 25px;
}
div.container {
  padding-top: 400px;
  width: 100%;
}
</style>
