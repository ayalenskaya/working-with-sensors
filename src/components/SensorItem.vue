<template>
    <div class="sensor-list__item" :class="{ 'sensor-list__item--selected': selected }">
        <p class="sensor-list__item--name">{{ sensor.name }}</p>
        <button @click="$emit('details', sensor)"
            class="sensor-list__item-button sensor-list__item-button--details">Подробнее</button>
        <button @click="$emit('remove', sensor.sensor_id)"
            class="sensor-list__item-button sensor-list__item-button--remove">Удалить</button>
        <SensorDetails v-if="selected" :sensor="sensor" />
    </div>
</template>
  
<script>
import SensorDetails from '@/components/SensorDetails.vue';

export default {
    components: {
        SensorDetails
    },
    props: {
        sensor: {
            type: Object,
            required: true
        },
        selected: {
            type: Boolean,
            default: false
        }
    }
};
</script>

<style scoped> 
.sensor-list__item {
     display: flex;
     flex: 0 0 33.333%;
     max-width: 365px;
     height: 350px;
     flex-direction: column;
     border: 1px solid #888;
     padding: 20px;
     border-radius: 10px;
     box-shadow: 0 6px 10px 0 rgba(0, 0, 0, 0.02),
         0 20px 30px 0 rgba(0, 0, 0, 0.04);
     min-width: 365px;
 }

 .sensor-list__item--name {
     font-size: 30px;
     margin-bottom: 15px;
 }
</style>