<template>
    <div>
        <form v-on:submit.prevent="getWeather">
        <label>Enter Zipcode: </label>
        <input type="text" v-model="zipcode"/>
        <input type="submit" value="Get Weather" />
        </form>
        <div v-show="lat !=''">
            You are in {{ name }}.
            Your latitude is {{ lat }} and longitude is {{ lon }}
        </div>

        <div v-show="temp !=''">
            Today's temp is {{ temp }} and it feels like {{ feelsLike }}
            {{ description }}
            <div>
            <img :src="image" alt=""/>
            </div>
        </div>
    </div>
</template>

<script>
import service from '../services/WeatherService.js';
export default{
    data() {
        return {
            zipcode: "",
            lat: "",
            lon: "",
            name: "",
            temp: "",
            feelsLike: "",
            description: "",
            id: "",
            icon: "",
            image: "",
        };
    },
    methods: {
        getWeather(){
            service.getWeather(this.zipcode)
            .then((response) => {
                console.log(response.data);
                this.temp = response.data.main.temp;
                this.feelsLike = response.data.main.feels_like;
                this.description = response.data.weather[0].description;
                this.icon = response.data.weather[0].icon;
                this.image = "https://openweathermap.org/img/wn/" + this.icon + "@2x.png";
                this.id = response.data.id;
                })
        }
    }
};
</script>


<style scoped>
output, form{
  display: block;
  margin: 20px;
}
img {
  margin: 20px;
  border: 2px solid black;
  background-color: lightgray;
}

</style>