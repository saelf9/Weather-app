<template>
  <div id="app" :class="weatherClass">
    <main>
      <GeolocatedHeader :location="location" :currentDate="currentDate" />

      <div class="search-box">
        <input
            type="text"
            class="search-bar"
            placeholder="Search..."
            v-model="query"
            @keypress="fetchWeather"
        />
      </div>

      <ForecastComponent :forecast="weather" v-if="weather.length > 0" />
    </main>
  </div>
</template>

<script>
import GeolocatedHeader from './components/header.vue';
import ForecastComponent from './components/forecast.vue';

export default {
  name: 'App',
  components: {
    GeolocatedHeader,
    ForecastComponent
  },
  data() {
    return {
      api_key: '9a05695d2ceb46c89a7976c60b9165b0',
      url_base: 'https://api.weatherbit.io/v2.0/forecast/daily',
      query: '',
      weather: [],
      location: '',
      currentDate: '',
    };
  },
  computed: {
    weatherClass() {
      return (this.weather.length > 0 && this.weather[0].temp > 16) ? 'warm' : '';
    }
  },
  methods: {
    fetchWeather(e) {
      if (e.key === 'Enter') {
        fetch(`${this.url_base}?city=${this.query}&days=5&units=metric&key=${this.api_key}`)
            .then(res => res.json())
            .then(data => this.setResults(data.data));
      }
    },
    setResults(results) {
      this.weather = results.map(day => ({
        date: new Date(day.datetime).toLocaleDateString('en-US', {weekday: 'long', month: 'long', day: 'numeric'}),
        temp: Math.round(day.temp),
        weather: day.weather.description
      }));
    },
    setCurrentLocation() {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition((position) => {
          const latitude = position.coords.latitude;
          const longitude = position.coords.longitude;

          fetch(`${this.url_base}?lat=${latitude}&lon=${longitude}&days=5&units=metric&key=${this.api_key}`)
              .then(res => res.json())
              .then(data => {
                // Set location and weather data
                this.location = data.city_name; // Assuming data structure includes city_name
                this.currentDate = new Date().toLocaleDateString('en-US', {weekday: 'long', month: 'long', day: 'numeric'});
                this.weather = data.data.map(day => ({
                  date: new Date(day.datetime).toLocaleDateString('en-US', {weekday: 'long', month: 'long', day: 'numeric'}),
                  temp: Math.round(day.temp),
                  weather: day.weather.description
                }));
              })
              .catch(error => {
                console.error('Error fetching weather:', error);
              });
        }, (error) => {
          console.error('Error getting geolocation:', error);
        });
      } else {
        console.error('Geolocation is not supported by this browser.');
      }
    }
  },
  mounted() {
    this.setCurrentLocation();
  }
};
</script>

<style>
#app {
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
  background-color: cornflowerblue;
}

#app.warm {
  background-color: tomato;
}

main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}
</style>
