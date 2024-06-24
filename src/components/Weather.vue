<template>
  <q-page class="column items-center justify-center q-pa-md bg-image">
    <div class="input-section">
      <q-input square standout bottom-slots v-model="newLocation" label="Enter Location" counter>
        <template v-slot:prepend>
          <q-icon name="place" />
        </template>
        <template v-slot:append>
          <q-icon name="close" @click="newLocation = ''" class="cursor-pointer" />
        </template>
        <template v-slot:hint>
          Enter a city name to get weather details.
        </template>
      </q-input>
      <q-btn
        size="35px"
        round
        color="primary"
        icon="map"
        @click="addWeatherWidget"
      />
    </div>
    <div class="widgets-section">
      <div
        v-for="(weather, index) in weatherStore.weatherWidgets"
        :key="index"
        :class="['weather-widget', { 'flipped': weather.showDetails }]"
      >
        <div class="card-front">
          <div class="header">
            <q-icon :name="getWeatherIcon(weather.weather[0].main)" size="100px" color="accent" />
            <div class="location">
              <h2>{{ weather.name }}</h2>
              <div class="country-info">
                <p>{{ weather.sys.country }}</p>
                <country-flag :iso="weather.sys.country.toLowerCase()" size="small" />
              </div>
            </div>
          </div>
          <div class="temperature">
            <p>{{ convertTemp(weather.main.temp) }}</p>
          </div>
          <q-btn
            class="center-button"
            @click="toggleDetails(index)"
            label="More Details"
            color="primary"
          />
        </div>
        <div class="card-back">
          <div class="details">
            <p><strong>Condition:</strong> {{ weather.weather[0].description }}</p>
            <p><strong>Wind:</strong> {{ weather.wind.speed }} m/s</p>
            <p><strong>Humidity:</strong> {{ weather.main.humidity }}%</p>
            <p><strong>Feels Like:</strong> {{ convertTemp(weather.main.feels_like) }}</p>
            <p><strong>Pressure:</strong> {{ weather.main.pressure }} hPa</p>
          </div>
          <q-btn
            class="btn-back"
            @click="toggleDetails(index)"
            label="Back"
            color="secondary"
          />
        </div>
        <q-btn
          @click="removeWidget(index)"
          label="Remove Widget"
          color="negative"
        />
      </div>
    </div>
  </q-page>
</template>

<script setup>
import { ref } from "vue";
import { QPage, QInput, QBtn, QIcon } from "quasar";
import { useWeatherStore } from "../stores/weatherStore";
import CountryFlag from "vue-country-flag-next";

const weatherStore = useWeatherStore();

const newLocation = ref("");

const weatherIconMapping = {
  Clear: "wb_sunny",
  Clouds: "cloud",
  Rain: "grain",
  Drizzle: "grain",
  Thunderstorm: "flash_on",
  Snow: "ac_unit",
  Mist: "blur_on",
  Smoke: "blur_on",
  Haze: "blur_on",
  Dust: "blur_on",
  Fog: "blur_on",
  Sand: "blur_on",
  Ash: "blur_on",
  Squall: "blur_on",
  Tornado: "toys",
};

const convertTemp = (tempInCelsius) => {
  return `${tempInCelsius.toFixed(1)} °C / ${(
    (tempInCelsius * 9) / 5 +
    32
  ).toFixed(1)} °F`;
};

const getWeatherIcon = (weatherMain) => {
  return weatherIconMapping[weatherMain] || "wb_cloudy";
};

const addWeatherWidget = async () => {
  await weatherStore.fetchWeather(newLocation.value);
  newLocation.value = "";
};

const toggleDetails = (index) => {
  weatherStore.weatherWidgets[index].showDetails = !weatherStore.weatherWidgets[index].showDetails;
};

const removeWidget = (index) => {
  weatherStore.removeWidget(index);
};
</script>

<style scoped>
.input-section {
  display: flex;
  align-items: center;
  gap: 12px;
  margin-bottom: 20px;
  padding: 12px;
  background-color: #f4f4f4;
  border-radius: 8px;
}

.widgets-section {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  justify-content: center;
  padding: 20px;
}

.weather-widget {
  position: relative;
  width: 280px;
  height: 380px;
  background: #ffecb3;
  border-radius: 15px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  overflow: hidden;
  perspective: 1000px;
  transition: transform 0.5s;
}

.weather-widget.flipped .card-front {
  transform: rotateY(180deg);
}

.weather-widget.flipped .card-back {
  transform: rotateY(0);
}

.card-front, .card-back {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  transition: transform 0.5s;
}

.card-front {
  transform: rotateY(0);
}

.card-back {
  transform: rotateY(-180deg);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background: #fff;
  border-radius: 15px;
}

.weather-widget .header {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 20px;
  padding-top: 50px;
  background: #ffd54f;
  border-radius: 15px 15px 0 0;
}

.weather-widget .location {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.weather-widget .location h2 {
  font-size: 1.5em;
  margin: 0;
  color: #333;
  font-weight: bold;
}

.weather-widget .location .country-info {
  display: flex;
  align-items: center;
  gap: 1px;
}

.weather-widget .location .country-info p {
  margin: 0;
  font-size: 1.3em;
  font-weight: bold;
  color: #666;
}

.weather-widget .temperature {
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 1.4em;
  font-weight: bold;
  margin: 10px 0;
  color: #333;
}

.weather-widget .details p {
  font-size: 1.3em;
  margin: 5px 0;
  color: #555;
}

.weather-widget .details p strong {
  color: #333;
}

.weather-widget .center-button {
  display: flex;
  justify-content: center;
  width: 100%;
  margin-top: 10px;
}

.country-info img {
  width: 22px;
  height: auto;
  border-radius: 50%;
}

.btn-back {
  margin-top: 40px;
}
</style>
