<template>
    <div class="sensor-list__details">
        <p v-if="sensor.sensor_id" class="sensor-list__details-id">№ {{ sensor.sensor_id }}</p> 
        <p v-if="sensor.temperature" class="sensor-list__details-temperature">Температура: {{ sensor.temperature }}°</p>
        <p v-if="sensor.humidity" class="sensor-list__details-humidity">Влажность: {{ sensor.humidity }} %</p>
    </div>
</template>

<script>
export default {
    props: {
        sensor: {
            type: Object,
            required: true
        }
    }
};
</script>

