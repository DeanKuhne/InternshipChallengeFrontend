<template>
	<div align="center" :style="{'background-image':'url(https://vuejs.org/images/logo.png', 'background-size':'auto', 'background-repeat':'no-repeat'}">
    	<h1> Vue.js and Express.js Weather Services: </h1>
    	<h3> Once a latitude and longitude are entered, your weather will appear in the fields below </h3>
    	<br> </br>
    	<h3> The weather in {{Weather.location }}, {{ Weather.country }} </h3>
    	<body> Feels like {{ Weather.percievedTemperature }}°F, despite actually being {{ Weather.actualTemperature }}°F </body>
    	<body> Forecast: {{Weather.forecast }} </body>
    	<div class='search-box'>
    		<input type="text" class="search-bar" placeholder="Enter Latitude Here" v-model="latitude"/>
    		<input type="text" class="search-bar" placeholder="Enter Longitude Here" v-model="longitude"/>
    	</div>
    	<button type="button" @click="verifyInputFields"> What's the weather? </button>

    	<br> </br>
		<br> </br>
    	<br> </br>
		<body> Moscow's Latitude and Longitude: 55.7558 | 37.6173 </body>
		<body> Schenectady's Latitude and Longitude: 42.8142 | -73.9396 </body>
		<body> NYC's Latitude and Longitude: 40.7128 | -74.0060 </body>
	</div>
</template>

<script>
import axios from 'axios';

export default {
	name: 'HelloVue',
	data () {
    	return {
    		Weather : {
				location: 'Clifton Park',
				country: 'US',
				percievedTemperature: '67.3',
				actualTemperature: '72.8',
				forecast: 'absolutely perfect'
			},
    		latitude : '',
    		longitude : ''
    	}
  	},
  	methods: {
    	verifyInputFields () {
      		if(this.latitude != '' && this.longitude != ''){ //is not null test
				if(!isNaN(this.latitude) && !isNaN(this.longitude)){ //is numeric test
					if(this.latitude >= -90 && this.latitude <= 90 && this.longitude >= -180 && this.longitude <= 180){
						this.acquireForecast();
					}else{ //our lat or long is out of bounds, so notify user of the issue
              			alert("A proper Latitude is between -90 and 90. A proper Longitude is between -180 and 180. Please correct this before retrying.");
					}
				}else{//our lat or long is not numeric, so notify user of the issue
        			alert("Numeric values for lat/long only. Please correct this before retrying.");
				  }
      		}else{ //our lat or long is a null, so notify user of an ambiguous issue
        		alert("Please enter values before attempting to retrieve a forecast.");
      		}
    	},
		acquireForecast () {
			axios.get(`http://localhost:3000?latitude=${this.latitude}&longitude=${this.longitude}`)
			.then((response) => {
				console.log(response.data);
				this.Weather = response.data;
			})
			.catch((error) => {
				console.log(error);
			})
		}
  	}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
