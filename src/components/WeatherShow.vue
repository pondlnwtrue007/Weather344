<script setup>
import { useWeatherStore } from '@/stores/weather';
const weatherStore = useWeatherStore();

const thaiMonths = [
  "มกราคม", "กุมภาพันธ์", "มีนาคม",
  "เมษายน", "พฤษภาคม", "มิถุนายน",
  "กรกฎาคม", "สิงหาคม", "กันยายน",
  "ตุลาคม", "พฤศจิกายน", "ธันวาคม"
];

function formatDtTxt(dt_txt) {
  const parsedDate = new Date(dt_txt);
  const day = parsedDate.getDate();
  const month = thaiMonths[parsedDate.getMonth()];
  const year = parsedDate.getFullYear() + 543; 
  return `วันที่ ${day} เดือน ${month} พ.ศ. ${year}`;
}

function formatTimeTxt(dt_txt) {
  const parsedDate = new Date(dt_txt);
  const hours = parsedDate.getHours();
  const minutes = parsedDate.getMinutes().toString().padStart(2, '0');;
  return `เวลา ${hours}.${minutes} น.`;
}

function groupDataByDate(data) {
  const NewData = {};

  data.forEach(item => {
    const date = item.dt_txt.split(' ')[0]; 
    if (!NewData[date]) {
      NewData[date] = [];
    }
    NewData[date].push(item);
  });

  return NewData;
}

const yourData = weatherStore.weatherData.list;
const NewData = groupDataByDate(yourData);


</script>

<template>
    <div class="frameoutline">
        <div class="frame">
            <div class="card text-center mb-3 mt-4" style="width: 80vw; margin: auto;  background-color:  rgba(255, 255, 255,0.8 ); border: solid 2px rgba(255, 255, 255, 0.8)">
                <div class="card text-center mb-3 mt-4" style="width: 80%; margin: auto;  background-color:  rgba(255, 255, 255,0.8 ); border: solid 2px rgba(148, 147, 147, 0.5)">
                    <div class="card-body">
                        <h2 class="card-title">พยากรณ์อากาศ 5 วัน ของ {{ weatherStore.weatherData.city.name }}</h2>
                    </div>
                </div>
                <div class="card text-center mb-3 mt-2 dayloop" style="width: 95%; margin: auto;  background-color:  rgba(255, 255, 255,0.5 ); border: solid 2px rgba(148, 147, 147, 0.5);" v-for="(date, dateData) in NewData" :key="date">
                    <h3 class="pt-2 pb-2" style="border-radius: 5% 5% 0% 0%; background-color: rgba(255, 255, 255,0.5 ); border-bottom: solid 2px rgb(148, 147, 147);">{{formatDtTxt(dateData)}}</h3>
                    <div class="card-body ">
                        <div class="datashowfram mt-1" style="display: flex;" >
                            <div class="row row-cols-4" style="width: 95%; margin: auto;">
                                <div class="col mb-4 timeloop" v-for="(item, index) in date" :key="index">
                                    <div class="card" style="background-color:  rgba(255, 255, 255,0.5 );">
                                        <h5 class="card-text mt-2">{{ formatTimeTxt(item.dt_txt) }} </h5>
                                        <div class="imgframe">
                                            <img :src="'https://openweathermap.org/img/wn/' + item.weather[0].icon + '@2x.png'" class="card-img-top" alt="weather" style="width: 100px;">
                                        </div>
                                        <div class="card-body" style="text-align: start;">
                                            <span style="font-size: 150%; font-weight:600;">{{ Math.round(item.main.temp) }} °C  </span> 
                                            <span style="font-size: 100%; font-weight:500;" class="card-text">{{ item.weather[0].description }} </span>
                                            <p>รู้สึก {{ Math.round(item.main.feels_like) }} °C </p>
                                            <p>ต่ำสุด {{ Math.round(item.main.temp_min) }} °C, สูงสุด {{ Math.round(item.main.temp_max) }} °C </p>
                                            <hr> 
                                            <p class="card-text">ความชื้น : {{ item.main.humidity }}%</p>
                                            <p class="card-text">ความกดอากาศ : {{ item.main.grnd_level }} มิลลิบาร์</p>
                                            
                                        </div>
                                    </div>
                                </div>

                            </div>
                        </div>
                    </div>


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

p {
    margin: 0%;
}
.frameoutline{
    height: auto;
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