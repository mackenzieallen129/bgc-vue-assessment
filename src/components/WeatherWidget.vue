<template>
    <div class="weather-widget">
        <h2>{{ locationName }}</h2>
        <div v-if="loading">
            <p>Loading...</p>
        </div>
        <div v-else-if="error">
            <p>{{ errorMessage }}</p>
        </div> 
        <div v-else>
            <p class="temperature">Current Temperature: {{ weather.temperature }}°{{ temperatureUnit }}</p>
        </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
    data() {
        return {
            locationName: 'Boston, MA',
            temperatureUnit: 'C',
            loading: true,
            error: ''
        };
    },
    mounted() {
        this.getLocalWeather();
    },
    methods: {
        async getLocalWeather() {
            const apiKey = 'kLMmv1mzAVsMUWA84cRVZuLn7CwSX4HZ';
            const latitude = '42.3601';
            const longitude = '-71.0589';
            const url = `https://api.tomorrow.io/v4/timelines?location=${latitude},${longitude}&fields=temperature&units=metric&timesteps=current&apikey=${apiKey}`;

            try {
                const response = await axios.get(url);
                const { data } = response;
                if (response.status !== 200 || (data.errors && data.errors.length > 0)) {
                    throw new Error('Failed to fetch weather data');
                }
                const currentWeather = data.data.timelines[0].intervals[0].values;
                this.weather = {
                    temperature: currentWeather.temperature,
                };
            } catch (error) {
                this.error = 'Failed to fetch weather data. Please try again later.';
            } finally {
                this.loading = false;
            }
        }
    }
};
</script>