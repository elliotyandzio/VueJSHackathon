<template>
  <div class="">
    <h1 class="title is-1 has-text-centered"> FOODOO (to-the) ROO 🍔 🐓 🇬🇧 🇵🇱 🇮🇳 🇦🇺</h1>
    <div class="hello container">
      <input ref="autocomplete"
             placeholder="Enter your address"
             type="text"
             class="input"
      />
      <div v-if="this.weather.timezone" class="columns">
        <div class="column">
          <h1 class="title is-2">Todays Weather</h1>
          <p>{{this.weather.timezone}}</p>
          <p>{{ this.weather.currently.summary }}</p>
          <p> {{(parseFloat((this.weather.currently.temperature)-32)*5/9).toFixed(1)}} Degrees </p>
        </div>
          <div class="column" v-if="this.weather.timezone">
            <h1 class="title is-2">Weather Forecast</h1>
            <p > {{this.weather.daily.summary}} </p>
          </div>
      </div>
      <div v-if="this.weather.timezone" class="columns is-centered">
        <div class="column has-text-centered">
          <img v-if="this.weather.currently.summary === 'Mostly Cloudy'" src="https://media.self.com/photos/5a625de37b39d91320a3ab0c/4:3/w_728,c_limit/cloudy-pee.jpg" />
          <img v-if="this.weather.currently.summary === 'Overcast'" src="https://ak6.picdn.net/shutterstock/videos/2615726/thumb/1.jpg?i10c=img.resize(height:160)" />
          <img v-if="this.weather.currently.summary === 'Sunny'" src="http://magarticles.magzter.com/articles/4212/195779/582eb9d4ed894/Sunny-Side-Trump.jpg" />
          <img v-if="this.weather.currently.summary === 'Rainy'" src="http://thechronicleherald.ca/sites/default/files/imagecache/ch_article_main_image/articles/B97693421Z.120170609150556000GFHHF80P.11.jpg" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import axios from 'axios'

  export default {
    data() {
        return {
          weather: {}
        };
      },

    mounted() {
      var autocomplete = new google.maps.places.Autocomplete(
      /** @type {!HTMLInputElement} */(this.$refs.autocomplete),
      {types: ['geocode']});

      autocomplete.addListener('place_changed', () => {
        let place = autocomplete.getPlace();
        let lat = place.geometry.location.lat();
        let lon = place.geometry.location.lng();

        console.log(`The coordinates ${lat}, ${lon}`);

        axios
        .get(`https://cors-anywhere.herokuapp.com/https://api.darksky.net/forecast/a942b21cd38241b71065d147d4852fd1/${lat},${lon}`)
        .then(res => this.weather = res.data);
      });
    }
  }

  // https://cors-anywhere.herokuapp.com/https://api.darksky.net/forecast/
  // https://cors-anywhere.herokuapp.com/https://api.darksky.net/forecast/a942b21cd38241b71065d147d4852fd1/42.3601,-71.0589

</script>
