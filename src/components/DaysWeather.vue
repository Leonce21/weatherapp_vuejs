<template>
 
  <div class="days-tab text-center justify-content-center">
    <div v-if="loading" class="loading">Loading...</div>
    <ul v-else class="p-0">
      <li v-for="day in forecast" :key="day.date" class="li_active">
        <div class="py-3"><img :src="day.iconUrl"></div>
        <div class="py-3">{{ getDayName(day.date) }}</div>
        <div class="py-3">{{day.temperature}}&deg;C</div>
      </li>
      
    </ul>
  </div>

</template>
  
<script>
import axios from 'axios'
import moment from 'moment'

  export default(await import('vue')).defineComponent({
    name: 'App',
    components: {
      
    },
    props:{
      cityname : String
    },

    data(){
      return {
        forecast: [],
        loading: true

      }
    },

    mounted(){
      this.fetchWeatherData()
    },

    methods: {
      async fetchWeatherData() {
        const apikey = 'your api key'
        const city = this.cityname
        const apiUrl = `https://api.openweathermap.org/data/2.5/forecast?q=${city}&units=metric&appid=${apikey}`

        await axios.get(apiUrl).then(Response => {
          const forecastData = Response.data.list
          const filteredData = forecastData.map(item => {
            return {
              date: moment(item.dt_txt.split(' ')[0]),
              temperature: Math.round(item.main.temp),
              description: item.weather[0].decription,
              iconUrl:  `https://api.openweathermap.org/img/w/${item.weather[0].icon}.png`
            };

          }).reduce((acc, item) => {

            if(!acc.some(day => day.date.isSame(item.date, 'day'))){
              acc.push(item);
            }
            return acc;

          }, []).slice(1, 5);

          console.log(filteredData, "working")
          this.forecast = filteredData
          this.loading = false

        }).catch(error => {

          console.error('Error fetching weather data: ', error)
          this.loading = false

        })
      
      },

      getDayName(date){
        return date.format('ddd')
      }

    }
    

  })

</script>

<style>
  .days-tab{
    box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2),0 6px 20px 0 rgba(0,0,0,0.19);
    border-radius: 20px;
    margin:200px 10px 10px 10px;
  }

  .loading{
    color: #fff;
  }

  ul{
    margin:0;
    height: 200px;
  }

  .days-tab ul li{
    display:inline-block;
    list-style: none;
    height: 100%;
    width: 21%;
    max-width: 21%;
    font-size: 1vw;
    line-height: 1.2;
  }

  span{
    display: block;
    margin-bottom: 5px;
    font: 100% sans-serif;
    height: 35px;
  }

  .li_active{
    background: #253d5c;
    color:#222831;
    border-radius: 10px;
    margin-top:200px ;
    margin: 0.5rem;
    box-shadow: #203249;
    color:#fff;
    font-weight: 600;
  }

  .days-tab ul .li_active:hover{
    background: #203249;
  }

  .li_active_temp{
    display:inline-block;
    background-color: #222831;
    color:#ffffff;
    transition:backgroud-color 0.5s;
    border-radius: 10px;
  }

  .li_active_temp:hover{
    transform: scale(1.2);
    transition: transform 0.1s ease;
    background: #fff;
    color: #191a1f;
    border-radius: 10px;
  }
  
</style>