<template>
  <div class="weather-wrap">
    <div v-for="(day, index) in forecast" :key="index" class="weather-box">
      <div class="date">{{ day.date }}</div>
      <div class="icon"><i :class="getWeatherIcon(day.weather)"></i></div>
      <div class="temp">{{ day.temp }}°C</div>
      <div class="weather">{{ day.weather }}</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ForecastComponent',
  props: {
    forecast: Array
  },
  methods: {
    getWeatherIcon(weather) {
      const lowerCaseWeather = weather.toLowerCase();

      if (lowerCaseWeather.includes('clear') || lowerCaseWeather.includes('sun')) {
        return 'fas fa-sun';
      } else if (lowerCaseWeather.includes('few clouds') || lowerCaseWeather.includes('scattered clouds') || lowerCaseWeather.includes('broken clouds')) {
        return 'fas fa-cloud-sun';
      } else if (lowerCaseWeather.includes('cloud') || lowerCaseWeather.includes('overcast')) {
        return 'fas fa-cloud';
      } else if (lowerCaseWeather.includes('rain') || lowerCaseWeather.includes('shower')) {
        return 'fas fa-cloud-rain';
      } else if (lowerCaseWeather.includes('thunderstorm') || lowerCaseWeather.includes('storm')) {
        return 'fas fa-bolt';
      } else if (lowerCaseWeather.includes('snow')) {
        return 'fas fa-snowflake';
      } else if (lowerCaseWeather.includes('fog') || lowerCaseWeather.includes('mist')) {
        return 'fas fa-smog';
      } else {
        return 'fas fa-question';
      }
    }
  }
}
</script>

<style>
.weather-wrap {
  display: flex;
  justify-content: space-around;
}

.weather-box {
  flex: 1 1 20%;
  text-align: center;
  padding: 10px;
  background-color: rgba(255, 255, 255, 0.2);
  border-radius: 10px;
}

.date, .temp, .weather {
  color: #FFF;
  margin-bottom: 5px;
}

.temp {
  font-size: 1.5rem;
  font-weight: bold;
}

.weather {
  font-style: italic;
}

.icon {
  font-size: 2rem;
}
</style>
