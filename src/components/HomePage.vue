<script>
import MapSection from "./MapSection.vue"
import axios from 'axios'

export default {
  name: 'HomeComponent',
  components: {
    MapSection
  },
  data() {
    return {
      apidata: [],
      lon: 0,
      lat: 0,
      city: [],
      latChanger: 0,
      longChanger: 0,
      Day: ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"],
      testAry: []
    }
  },
  methods: {
    dayGetter(ind) {
      const date = new Date();
      const day = date.getDay();
      let i = day + ind;

      if (i > 6) {
        i -= 7;
        return this.Day[i];
      } else {
        return this.Day[i];
      }
    },
    handleViewMoreClick() {
      console.log("button pressed")
      if (this.$refs.menuSectionRef) {
        this.$refs.menuSectionRef.scrollIntoView({behavior: 'smooth'});
      }

    },
    async getData() {
      try {
        const lond = this.$store.state.long
        const latd = this.$store.state.lat
        const apikey = import.meta.env.VITE_WEATHER_KEY
        console.log(latd, lond)
        const res = await axios.get(`http://api.openweathermap.org/data/2.5/forecast?appid=${apikey}&lat=${latd}&lon=${lond}&cnt=7&units=metric&lang=en`);
        if (res) {
          this.apidata = res.data.list;
          this.apidata = res.data.list;
          this.city = res.data.city.name
        }

      } catch (e) {
        console.log(e.message);
      }
    },

  },
  created() {
    this.getData();
  },
  computed: {
    laTo() {
      return (this.latChanger = this.$store.state.lat)
    }
  },
  watch: {
    laTo() {
      this.getData();
    }
  }
};
</script>

<template>

  <div>
    <!--    first page section start-->
    <div class=" w-[100%] h-[95vh] ">
      <div class="w-[100%]  h-[70vh] p-2 flex justify-center relative"><img src="../assets/WeatherForcast.svg"
                                                                            alt="Vue logo"
                                                                            class="h-[100%] w-[100%] absolute">


      </div>
      <div class="flex w-full flex-col justify-center items-center text-center">
        <h1 class="font-bold text-3xl pb-3">Welcome to Weather Walley</h1>

        <button class="bg-[#79A3FF] py-2 px-8 rounded-lg shadow-lg font-bold " @click="handleViewMoreClick">Get Weekly
          Updates
        </button>
      </div>
    </div>
    <!--    first page section end-->
    <div class="p-[3px] bg-[#ACD4F7]">

    </div>

    <!--    map section start-->
    <div ref="menuSectionRef">
    <MapSection />
    </div>
    <!--    map section end-->
    <div class="p-[3px] bg-[#ACD4F7]">

    </div>
  </div>

  <!--  Weather List Render start-->
  <div class="max-w-[100%]  p-4 flex justify-center my-8">
    <div class="border  rounded-lg shadow-lg w-[85%] p-8">

      <div class="w-[100%]">
        <div class=" flex justify-center text-3xl font-bold">Weekly Updates of
          <!--          {{ this.city.length === 0 ? "Islamabad" : this.city }}-->
          {{ this.city.length === 0 ? "Islamabad" : this.$store.state.city }}
        </div>
        <div class="py-4"><p class="text-2xl flex justify-center ">Stay One Step Ahead with Our Comprehensive Weather
          Forecasts – Know What's Coming Your Way</p></div>
      </div>
      <ul>
        <li v-for="(item, index) in apidata" :key="item.dt" class="my-4 ">
          <div class="border-4 border-[#ACD4F7] rounded-lg shadow-lg py-5">
            <div class="w-[100%] flex justify-center"><p class="font-bold text-2xl ">{{ dayGetter(index) }}</p></div>
            <div class="flex w-[100%]">
              <div class="w-[90%] pt-4">
                <p class="font-[400] text-xl font-bold">Daily Report : </p>
                <div class="flex justify-around py-4">
                  <div>
                    <p class="font-[400] text-xl">Weather: {{ item.weather[0].main }}</p>
                    <p class="font-[400] pt-1 text-xl">Description: {{ item.weather[0].description }}</p>
                  </div>
                  <div class="">
                    <p class="font-[400] text-xl">Humidity: {{ item.main.humidity }}</p>
                    <p class="font-[400] pt-1 text-xl">Temp: {{ item.main.temp }} C</p>
                  </div>
                </div>


              </div>
              <div class="w-[10%]">
                <img width="100%" height="100%" :src="`http://openweathermap.org/img/wn/${item.weather[0].icon}@2x.png`"
                     alt="Weather Icon">
              </div>
            </div>
          </div>
        </li>
      </ul>

      <div>
      </div>
    </div>
  </div>
  <!--  Weather List Render end-->
</template>
