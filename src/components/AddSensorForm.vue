<template>
  <div class="sensor-list__add-sensor">
    <input type="text" v-model="newSensor.name" placeholder="Название датчика" maxlength="18">
    <input type="text" v-model="newSensor.temperature" @input="filterInput('temperature')" placeholder="Температура">
    <input type="text" v-model="newSensor.humidity" @input="filterInput('humidity')" placeholder="Влажность">
    <button class="sensor-list__item-button" @click="saveSensor">Сохранить</button>
    <button class="sensor-list__item-button" @click="cancelAddSensor">Отмена</button>
  </div>
</template>

<script>
import { ref } from 'vue';

export default {
  setup(props, { emit }) {

    const newSensor = ref({
      name: '',
      temperature: '',
      humidity: ''
    });

    // Фильтрация ввода
    const filterInput = (field) => {
      newSensor.value[field] = newSensor.value[field].replace(/[^0-9]/g, '');
      if (newSensor.value[field].length > 2) {
        newSensor.value[field] = newSensor.value[field].slice(0, 2);
      }
    };

    // Сохранение датчика
    const saveSensor = () => {
      emit('save', {
        ...newSensor.value,
        temperature: newSensor.value.temperature ? Number(newSensor.value.temperature) : null,
        humidity: newSensor.value.humidity ? Number(newSensor.value.humidity) : null
      });
    };

    // Отмена добавления датчика
    const cancelAddSensor = () => {
      emit('cancel');
    };

    return {
      newSensor,
      filterInput,
      saveSensor,
      cancelAddSensor
    };
  }
};
</script>
<style scoped>
 .sensor-list__add-sensor {
   margin-bottom: 15px;
 }

 .sensor-list__add-sensor input {
   max-width: 500px;
   height: 40px;
 }

 input:not(:last-child) {
   margin-right: 15px;
 }

 @media screen and (max-width: 768px) {
   .sensor-list__add-sensor {
     display: flex;
     flex-direction: column;
   }
 }
</style>