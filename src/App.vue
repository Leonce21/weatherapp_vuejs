<template>
  <div class="app">
    <div class="header container h-100 p-5">
      <h1 class="mb-5">Weather App</h1>
      <div class="d-flex justify-content-center h-100">
        <div class="searchbar w-50 mx-2">
          <input type="text" class="input form-control" v-model="city" placeholder="Enter a city"> 
        </div>
        <button class="btn-search btn btn-primary" @click="searchWeather">Search <i class="fas fa-search"></i></button>
      </div>
    </div>
    <br>
    <Weather :city="city" v-if="showWeather"></Weather>
  </div>
</template>

<script>

import Weather from './components/Weather.vue';

  export default(await import('vue')).defineComponent({
    name: 'App',
    components: {
      Weather
    },

    data(){
      return{
        city: '',
        showWeather: false,
      }
    },

    methods: {
      async searchWeather () {
        this.showWeather = false;
        await this.$nextTick();
        this.showWeather = true;
      }
    }

  })

</script>

<style>
  body{
    background-color: #040405 !important;

  }
  .header{
    background-color: #212730;
    border-radius: 20px;
    color: #fff;
    text-align: center;
    font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, Helvetica, sans-serif, 'Roboto';
    margin-top: 1rem;
  }

  
</style>