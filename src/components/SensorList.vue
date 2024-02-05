<template>
  <div class="sensor-list">
    <h1 class="sensor-list__title">Список датчиков</h1>
    <button @click="addSensor" class="sensor-list__add-button">Добавить датчик</button>
    <AddSensorForm v-if="isAddingSensor" @save="saveNewSensor" @cancel="cancelAddSensor" />
    <div v-if="sensors.length" class="sensor-list__items">
      <SensorItem v-for="sensor in sensors" :key="sensor.sensor_id" :sensor="sensor" :selected="selectedSensor === sensor"
        @remove="removeSensor" @details="showSensorDetails(sensor)" />
    </div>
    <div v-else class="sensor-list__loading">
      Датчиков нет
    </div>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue';
import axios from 'axios';
import SensorItem from '@/components/SensorItem.vue';
import AddSensorForm from '@/components/AddSensorForm.vue';
import SensorDetails from '@/components/SensorDetails.vue';

export default {
  components: {
    SensorItem,
    AddSensorForm,
    SensorDetails
  },
  setup() {
    const sensors = ref([]);
    const selectedSensor = ref(null);
    const isAddingSensor = ref(false);

    // Получение данных о датчиках
    const fetchSensors = async () => {
      try {
        const storedSensors = localStorage.getItem('sensors');
        if (storedSensors) {
          sensors.value = JSON.parse(storedSensors);
        } else {
          const response = await axios.get('https://file.notion.so/f/f/216de177-6269-4124-912b-88f16b5e3e0f/c1b024a9-51d6-46c3-a87c-6518bd6f7ffd/events.json?id=e7861165-7c96-49e9-b07c-4019ded7c4c2&table=block&spaceId=216de177-6269-4124-912b-88f16b5e3e0f&expirationTimestamp=1707062400000&signature=G4-mWnX1Jds2d3GphNXAer8ZBDhYOjIckUWr9_lvHfg&downloadName=events.json');
          sensors.value = response.data;
          saveSensorsToLocalStorage();
        }
      } catch (error) {
        console.error('Ошибка при получении данных:', error);
      }
    };

    // Сохранение датчиков в локальном хранилище
    const saveSensorsToLocalStorage = () => {
      localStorage.setItem('sensors', JSON.stringify(sensors.value));
    };

    // Переключение флага на добавление датчика
    const addSensor = () => {
      isAddingSensor.value = true;
    };

    // Сохранение нового датчика
    const saveNewSensor = (newSensor) => {
      if (!newSensor.name) {
        alert('Название датчика должно быть заполнено');
        return;
      }
      const sensor = {
        sensor_id: Date.now(),
        name: newSensor.name,
        temperature: newSensor.temperature,
        humidity: newSensor.humidity
      };
      sensors.value.push(sensor);
      saveSensorsToLocalStorage();
      isAddingSensor.value = false;
    };

    // Отмена добавления датчика
    const cancelAddSensor = () => {
      isAddingSensor.value = false;
    };

    // Удаление датчика
    const removeSensor = (sensorId) => {
      const index = sensors.value.findIndex(sensor => sensor.sensor_id === sensorId);
      if (index !== -1) {
        sensors.value.splice(index, 1);
        saveSensorsToLocalStorage();
        if (selectedSensor.value && selectedSensor.value.sensor_id === sensorId) {
          selectedSensor.value = null;
        }
      }
    };

    // Отображение деталей датчика
    const showSensorDetails = (sensor) => {
      selectedSensor.value = selectedSensor.value === sensor ? null : sensor;
    };

    onMounted(fetchSensors);

    return {
      sensors,
      addSensor,
      saveNewSensor,
      cancelAddSensor,
      isAddingSensor,
      removeSensor,
      showSensorDetails,
      selectedSensor
    };
  }
};
</script>
  
<style scoped> 
.sensor-list__items {
   display: flex;
   flex-wrap: wrap;
   gap: 18px;
   border-radius: 10px;

 }

 .sensor-list__add-button {
   font-size: 30px;
   margin-bottom: 15px;
 }
</style>