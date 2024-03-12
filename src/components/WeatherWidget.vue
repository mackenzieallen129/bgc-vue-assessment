<template>
    <div class="weather-widget card bg-secondary text-white text-center mx-auto" style="max-width: 300px;">
        <div class="card-body" @mouseenter="showAdditionalData = true" @mouseleave="showAdditionalData = false">
            <h2>{{ locationName }}</h2>
            <div v-if="loading">
                <p>Loading...</p>
            </div>
            <div v-else-if="error">
                <p>{{ errorMessage }}</p>
            </div> 
            <div v-else>
                <p class="temperature">{{ weather.temperature }}°{{ temperatureUnit }}</p>
                <p class="weather"><i>Conditions:</i> {{ weather.weatherDescription }}</p>
            
                <div v-if="showAdditionalData" class="additional-data">
                    <p>Feels Like: <span class="value">{{ weather.temperatureApparent }}°{{ temperatureUnit }}</span></p>
                    <p>Humidity: <span class="value">{{ weather.humidity }}%</span></p>
                    <p>UV Index: <span class="value">{{ weather.uvIndex }}</span></p>
                    <p>Wind Speed: <span class="value">{{ weather.windSpeed }} mph</span></p>
                </div>
            </div>
        </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
    data() {
        return {
            locationName: 'Boston, MA',
            temperatureUnit: 'F',
            weather: null,
            loading: true,
            error: '',
            showAdditionalData: false
        };
    },
    mounted() {
        this.getLocalWeather();
    },
    methods: {
        async getLocalWeather() {
            const apiKey = 'kLMmv1mzAVsMUWA84cRVZuLn7CwSX4HZ';
            const cityName = 'boston';
            const url = `https://api.tomorrow.io/v4/timelines?location=${cityName}&fields=humidity,temperature,temperatureApparent,uvIndex,weatherCode,windSpeed&units=imperial&timesteps=current&apikey=${apiKey}`;

            try {
                const response = await axios.get(url);
                const { data } = response;
                if (response.status !== 200 || (data.errors && data.errors.length > 0)) {
                    throw new Error('Failed to fetch weather data');
                }
                const currentWeather = data.data.timelines[0].intervals[0].values;
                const temperature = parseFloat(currentWeather.temperature).toFixed(1);
                const temperatureApparent = parseFloat(currentWeather.temperatureApparent).toFixed(1);
                this.weather = {
                    humidity: currentWeather.humidity,
                    temperature: temperature,
                    temperatureApparent: temperatureApparent,
                    uvIndex: currentWeather.uvIndex,
                    weatherDescription: this.getWeatherDescription(currentWeather.weatherCode),
                    windSpeed: currentWeather.windSpeed
                };
            } catch (error) {
                this.error = 'Failed to fetch weather data. Please try again later.';
            } finally {
                this.loading = false;
            }
        },
        getWeatherDescription(weatherCode) {
            //got information on weather codes from tomorrow.io documentation
            const weatherDescriptions = {
                "0": "Unknown",
                "1000": "Clear, Sunny",
                "1100": "Mostly Clear",
                "1101": "Partly Cloudy",
                "1102": "Mostly Cloudy",
                "1001": "Cloudy",
                "2000": "Fog",
                "2100": "Light Fog",
                "4000": "Drizzle",
                "4001": "Rain",
                "4200": "Light Rain",
                "4201": "Heavy Rain",
                "5000": "Snow",
                "5001": "Flurries",
                "5100": "Light Snow",
                "5101": "Heavy Snow",
                "6000": "Freezing Drizzle",
                "6001": "Freezing Rain",
                "6200": "Light Freezing Rain",
                "6201": "Heavy Freezing Rain",
                "7000": "Ice Pellets",
                "7101": "Heavy Ice Pellets",
                "7102": "Light Ice Pellets",
                "8000": "Thunderstorm"
            };

            if (Object.prototype.hasOwnProperty.call(weatherDescriptions, weatherCode)) {
                return weatherDescriptions[weatherCode];
            } else {
                return "Mixed Conditions";
            }
        }
    }
};
</script>

<style scoped>
.temperature {
    font-size: 50px;
    font-weight: bold;
    color: lightskyblue;
}
.value {
    color: lightskyblue;
}
.additional-data {
  background-color: black;
  border-radius: 5px;
  padding: 10px;
  margin-top: 10px;
}
.additional-data p {
  margin: 0;
}
</style>