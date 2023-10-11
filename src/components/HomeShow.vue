<script setup>
import { RouterLink } from 'vue-router';
import { ref, watchEffect } from "vue";
import axios from "axios";
import { useWeatherStore } from '@/stores/weather';

const latitude = ref('18.89525429860656'); 
const longitude = ref('99.01108004859944'); 
const weatherStore = useWeatherStore();

watchEffect(() => {
  if (latitude.value && longitude.value) {
    showWeatherData();
  }
});

function WeatherData(url) {
  axios
    .get(url)
    .then((response) => {
      weatherStore.weatherData = response.data;
    })
    .catch((err) => {
      console.error(err);
    });
}

function showWeatherData() {
  const apiUrl = 'https://api.openweathermap.org/data/2.5/forecast';
  const apiKey = '4546b80f7b7b5d308f95442db543cb15';
  const lang = 'th';
  const units = 'metric';
  const url = `${apiUrl}?lat=${latitude.value}&lon=${longitude.value}&appid=${apiKey}&units=${units}&lang=${lang}`;
  WeatherData(url);
}



</script>

<template>
    <div class="frameoutline">
        <div class="frame">
            <div class="card text-center mb-3 mt-4" style="width: 80vw; margin: auto;  background-color:  rgba(255, 255, 255,0.8 );">
                <div class="card-body">
                    <img src="@/assets/CardImg.png" alt="cardimg" class="mb-3 mt-3" style="width: 30%;">
                    <h3 class="card-title">Weather Search</h3>
                    <div class="inputframe">
                        <div class="input-group" style="width: 70%; margin: auto;">
                            <span class="input-group-text">Latitude and Longitude</span>
                            <input type="text" aria-label="latitude" class="form-control" id="lat" v-model="latitude" placeholder="latitude">
                            <input type="text" aria-label="longitude" class="form-control" id="long" v-model="longitude" placeholder="longitude">
                        </div>
                    </div>
                    <p class="card-text mt-2 mb-2">กรอกตำแหน่งที่ต้องการค้นหา</p>
                    <RouterLink to="/weather" class="btn btn-primary mb-2" style="width: 50%;">ดูข้อมูล</RouterLink>
                </div>
            </div>
        </div>
    </div>

    <footer class="d-flex flex-wrap justify-content-between align-items-center py-3  border-top" style="background-color: rgba(148, 147, 147, 0.4);">
      <div class="col-md-4 d-flex align-items-center">
        <RouterLink to="/" class="mb-3 me-2 mb-md-0 text-body-secondary text-decoration-none lh-1">
          <svg class="bi" width="30" height="24"></svg>
        </RouterLink>
        <span class="mb-3 mb-md-0 text-body-secondary">© Thonthus Prangpetch 6504101343</span>
      </div>

      <ul class="nav col-md-2 justify-content-end list-unstyled d-flex mr-3">
        <li class="ms-3"><a class="text-body-secondary" href="https://www.instagram.com/ts_thus.pp/" target="_blank"><img src="@/assets/instagram.png" alt="ig" width="31" height="31"></a></li>
        <li class="ms-3"><a class="text-body-secondary" href="https://facebook.com/thonthus.prangpech/" target="_blank"><img src="@/assets/febe.png" alt="fb" width="30" height="30"></a></li>
        <svg class="bi" width="30" height="24"></svg>
      </ul>
    </footer>
</template>

<script>



  


</script>



<style scoped>

body{
    margin: 0%;
    padding: 0%;
}

.frameoutline{
height: 100vh;
background-color: transparent;
}

.frame{
    display: flex;
    background-color: transparent;
}

.inputframe{
    display: flex;
}

</style>