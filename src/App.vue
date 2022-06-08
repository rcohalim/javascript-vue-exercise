<template>
  <div id="app">
    <div class="container-lg container">
      <div class="title-container">
        <h1 class="title text-center">Good Morning!</h1>
      </div>
      
      <div class="form-container d-flex mt-3 mb-5 justify-content-end">
        <form class="search-location me-3" v-on:submit.prevent="getWeather">
          <input
            type="text"
            class="form-control text-muted form-rounded p-2 shadow-sm"
            placeholder="Search City..."
            v-model="citySearch"
            autocomplete="off"
          />
        </form>

        <input type="submit" class="btn btn-primary align-middle" value="Search" v-on:click.prevent="getWeather">
      </div>
      
      <div class="flex-row">
        <!-- <CityCard /> -->
      </div>

      <CityCard :city = "weather.cityName"/>

      <div class="card rounded p-5 my-3 shadow-lg back-card overflow-hidden">
        <h2 class="text-start">{{weather.cityName}}</h2>
        <div class="row mt-5">
          <h5 class="text-start">Today</h5>
            
          <div class="container-fluid mt-3">
            <div class="row">
              <!-- should be iterate -->
              <div class="col"> 
                <h5>3 AM</h5>
                <img src="" alt="">
                <h5>25C</h5>
              </div>
              <div class="col">2</div>
              <div class="col">3</div>
              <div class="col">4</div>
              <div class="col">5</div>
              <div class="col">6</div>
              <div class="col">7</div>
              <div class="col">8</div>
            </div>
          </div>

        </div>

        <div class="row mt-3">
          <h5 class="text-start">Tomorrow</h5>
          
          <div class="container-fluid mt-3">
            <div class="row">
              <div class="col">1</div>
              <div class="col">2</div>
              <div class="col">3</div>
              <div class="col">4</div>
              <div class="col">5</div>
              <div class="col">6</div>
              <div class="col">7</div>
              <div class="col">8</div>
            </div>
          </div>

        </div>
      </div>
    </div>
  </div>
</template>

<script>
import CityCard from './components/CityCard.vue';

export default {
  data() {
    return {
      citySearch: "",
      weather: {
        cityName:"Amsterdam",
        country:"NL",
        today_temp: 12,
        description: "Clouds everywhere",
      },
    };
  },
  methods: {
    getWeather: async function() {
      console.log(this.citySearch);
      const api_key = 'ceede1b6b3f225c1d5656cff0508e06a';
      const base_url = `https://api.openweathermap.org/data/2.5/forecast?q=${this.citySearch}&appid=${api_key}&units=metric`;

      // const icon_collection = `http://openweathermap.org/img/wn/${this.icon}@2x.png`;

      // fetch the JSON data from the base URL
      const response = await fetch(base_url);
      const data = await response.json();
      console.log(data);

      // assigning values to variables that are going to be used
      this.citySearch = "";
      this.weather.cityName = data.city.name;
      this.weather.country = data.city.country;

      // this.weather.icon = data.list.weather[0].icon;

      // const img

      // to retrieve the current date
      const today_dt = this.addDays(data.list[0].dt_txt, 0);

      // filter function to only includes weather's data from today's timestamp
      const data_today = data.list
        .filter(function (entry) {
          return (entry.dt_txt).includes(today_dt)
        });

      console.log(data_today);

      // to retrieve tomorrow's date
      const tomorrow_dt = this.addDays(data.list[0].dt_txt, 1);
      
      console.log(tomorrow_dt);
      
      // filter function to only includes weather's data from tomorrow's timestamp
      const data_tmr = data.list.filter(function (entry) {
        return (entry.dt_txt).includes(tomorrow_dt);
      });

      console.log(data_tmr);

    },

    // function to add day to retrieve tomorrow's date
    addDays: function(date, days) {
      var result = new Date(date);
      result.setDate(result.getDate() + days);

      const offset = result.getTimezoneOffset();
      result = new Date(result.getTime() - (offset*60*1000));
      return result.toISOString().split('T')[0];
    },
  },

  components: {
    CityCard
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

@import "./assets/style.css";

</style>
