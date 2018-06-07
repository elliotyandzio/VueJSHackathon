<template>
  <div class="hello">
    <input ref="autocomplete"
           placeholder="Enter your address"
           type="text"
           class="column is-half"
    />
  </div>
</template>

<script>
  import axios from 'axios'

  export default {
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
        .then(res => console.log(res.data))
      });
    }
  }

  // https://cors-anywhere.herokuapp.com/https://api.darksky.net/forecast/
  // https://cors-anywhere.herokuapp.com/https://api.darksky.net/forecast/a942b21cd38241b71065d147d4852fd1/42.3601,-71.0589

</script>
