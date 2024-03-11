<template>
    <div class="min-h-screen bg-gradient-to-r from-blue-300 to-blue-500 p-6 flex justify-center items-center">

        
        <div class="absolute top-0 right-0 m-4">
            <button class="bg-gray-900 text-white px-4 py-2 rounded-md" @click="login">Login</button>
        </div>

        <div>
            <div v-if="loading" class="text-lg text-white">Loading...</div>
            <div v-else class="bg-white rounded-xl shadow-xl p-6 max-w-sm">
    
                
                <div class="flex justify-between items-center mb-6">
                    <h1 class="text-2xl font-bold text-gray-700">{{ currentLocation }}</h1>
                    <span class="font-medium text-gray-500">{{ currentTime }}</span>
                </div>
  
                
                <div v-if="weather" class="text-center mb-4">
                    <p class="text-7xl font-semibold">{{ weather.main.temp.toFixed(0) }}°C</p>
                    <p class="text-gray-500 capitalize">{{ weather.weather[0].description }}</p>
                    <div class="flex justify-center mb-4">
                        <img :src="`http://openweathermap.org/img/w/${weather.weather[0].icon}.png`" alt="Weather Icon"
                            class="w-20 h-20">
                    </div>
                </div>
           

                <div class="text-center mb-4">
                    <button @click="toggleForecast"
                        class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">
                        {{ showForecast ? 'Hide' : 'Show' }} 5-day forecast
                    </button>
                </div>
                <!-- 5-day Forecast -->
                <div v-if="showForecast" class="pt-4">
                    <h2 class="text-center font-semibold mb-4 text-lg">5-day forecast</h2>
                    <div class="grid grid-cols-1 divide-y divide-gray-200">
                        <div v-for="(item, index) in forecast" :key="index"
                            class="py-3 flex justify-between items-center">
                            <span class="font-medium">{{ item.date }}</span>
                            <span class="text-sm text-gray-600">{{ item.temp }}°C / {{ item.description }}</span>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'WeatherApp',
    data() {
        return {
            currentLocation: 'Fetching location...',
            currentTime: '',
            weather: null,
            forecast: [],
            showForecast: false,
            loading: true,
            apiKey: 'a0e2d1547aac56611390778c1d47f037',
        };
    },
    mounted() {
        this.fetchWeather();
    },
    methods: {
        updateCurrentTime() {
            const now = new Date();
            this.currentTime = now.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });
        },
        toggleForecast() {
            this.showForecast = !this.showForecast;
        },
        fetchWeather() {
            if (navigator.geolocation) {
                navigator.geolocation.getCurrentPosition(
                    position => {
                        const lat = position.coords.latitude;
                        const lon = position.coords.longitude;
                        this.fetchCurrentWeather(lat, lon);
                        this.fetchForecast(lat, lon);
                    },
                    error => {
                        console.error("Geolocation is not supported by this browser.", error);
                        this.loading = false;
                    }
                );
            } else {
                console.error("Geolocation is not supported by this browser.");
                this.loading = false;
            }
        },
        fetchCurrentWeather(lat, lon) {
            const url = `https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&appid=${this.apiKey}&units=metric`;
            fetch(url)
                .then(response => response.json())
                .then(data => {
                    this.weather = data;
                    this.currentLocation = data.name;
                    this.updateCurrentTime();
                    setInterval(this.updateCurrentTime, 60000);
                });
        },
        fetchForecast(lat, lon) {
            const url = `https://api.openweathermap.org/data/2.5/forecast?lat=${lat}&lon=${lon}&appid=${this.apiKey}&units=metric`;
            fetch(url)
                .then(response => response.json())
                .then(data => {
                    this.forecast = data.list.filter((_, index) => index % 8 === 0).map(item => {
                        return {
                            date: new Date(item.dt_txt).toLocaleDateString([], { weekday: 'short', month: 'short', day: 'numeric' }),
                            temp: item.main.temp.toFixed(0),
                            description: item.weather[0].description,
                        };
                    });
                    this.loading = false;
                });
        },
        login() {

            
            console.log("Login button clicked");
        }
    },
};
</script>

<style>

</style>