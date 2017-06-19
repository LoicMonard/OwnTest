<template>
  <div class="hello">
    <h1>{{ msg }}</h1>

    <div id="myMap"></div>
  </div>
</template>

<script>
export default {
  name: 'hello',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }},
  mounted: function() {
    console.log("map: ", google.maps);
    this.map = new google.maps.Map(document.getElementById('myMap'), {
      center: {lat:61.180059, lng: -149.822075},
      scrollwheel: false,
      zoom: 4
    })
  }

}
</script>

<style scoped>
    #myMap {
    height:300px;
    width: 100%;
   }
</style>