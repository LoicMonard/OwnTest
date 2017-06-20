<template>
  <div id="app">
    <div>
      <input id="searchTextField" type="text" v-model="address">
      <button @click="addMarker()">Get loc details</button>
      <button @click="getLocPhoto()">Get loc photo</button>
      <img v-bind:src="this.response">
      <div id="myMap"></div>
    </div>
  </div>
</template>

<script>
import Map from './Map.vue'
import Navbar from './Navbar.vue'

export default {
  data () {
    return {
      address: '',
      response: '',
      markers: [],
      infoWindows: []
    }
  },
  methods: {
    getLoc() {
      var xmlHttp = new XMLHttpRequest();
      xmlHttp.open("GET", "https://maps.googleapis.com/maps/api/geocode/json?address="+this.address+"&key=AIzaSyCUEEzTpu8BzuQ2te4yY0gBzvQ6ax7w_wA"
, false);
      xmlHttp.send();
      this.response = xmlHttp.response;
      this.response = JSON.parse(this.response);
      console.log(this.response);
    },
    getLocPhoto() {
      this.response = "https://maps.googleapis.com/maps/api/streetview?size=600x400&location="+this.address+"&fov=120&heading=0&pitch=10&key=AIzaSyCUEEzTpu8BzuQ2te4yY0gBzvQ6ax7w_wA";
    },
    addMarker() {
      this.getLoc();
      var lat = this.response.results[0].geometry.location.lat;
      var lng = this.response.results[0].geometry.location.lng;
      var position = { lat: this.response.results[0].geometry.location.lat, lng: this.response.results[0].geometry.location.lng}

      var marker = new google.maps.Marker({
        position: position,
        map: this.map
      });

      var imgUrl = "https://maps.googleapis.com/maps/api/streetview?size=200x150&location="+this.response.results[0].formatted_address+"&fov=120&heading=0&pitch=10&key=AIzaSyCUEEzTpu8BzuQ2te4yY0gBzvQ6ax7w_wA";

      var infoWindow = new google.maps.InfoWindow({
        content: this.response.results[0].formatted_address + '<br><img src="'+imgUrl+'">'
      });

      marker.addListener('click', function() {
        infoWindow.open(this.map, marker);
      });

      // this.markers.push(marker);
      // var index = this.markers.indexOf(marker);
      
      // this.infoWindows.push(new google.maps.InfoWindow({
      //   content: this.response.results[0].formatted_address
      // }));

      // console.log(this.infoWindows);
    }
  },
  mounted: function() {
    var uluru = {lat:48.709444, lng: 2.492176};

    console.log("map: ", google.maps);
    this.map = new google.maps.Map(document.getElementById('myMap'), {
      center: {lat:48.709444, lng: 2.492176},
      scrollwheel: true,
      zoom: 14
    })
    var marker = new google.maps.Marker({
      position: uluru,
      map: this.map,
      title: 'Yerres'
    });
    var contentString = '<div id="content">'+
      '<div id="siteNotice">'+
      '</div>'+
      '<h1 id="firstHeading" class="firstHeading">Uluru</h1>'+
      '<div id="bodyContent">'+
      '<p><b>Uluru</b>, also referred to as <b>Ayers Rock</b>, is a large ' +
      'sandstone rock formation in the southern part of the '+
      'Northern Territory, central Australia. It lies 335&#160;km (208&#160;mi) '+
      'south west of the nearest large town, Alice Springs; 450&#160;km '+
      '(280&#160;mi) by road. Kata Tjuta and Uluru are the two major '+
      'features of the Uluru - Kata Tjuta National Park. Uluru is '+
      'sacred to the Pitjantjatjara and Yankunytjatjara, the '+
      'Aboriginal people of the area. It has many springs, waterholes, '+
      'rock caves and ancient paintings. Uluru is listed as a World '+
      'Heritage Site.</p>'+
      '<p>Attribution: Uluru, <a href="https://en.wikipedia.org/w/index.php?title=Uluru&oldid=297882194">'+
      'https://en.wikipedia.org/w/index.php?title=Uluru</a> '+
      '(last visited June 22, 2009).</p>'+
      '</div>'+
      '</div>';

    var infowindow = new google.maps.InfoWindow({
      content: contentString
    });
    marker.addListener('click', function() {
      infowindow.open(this.map, marker);
    });
  },
  components: {
    Navbar,
    Map
  }
}
</script>

<style>

body {
  height: 100%;
  width: 100%;
  padding: 0px;
  margin: 0px;
}

#myMap {
  height:100vh;
  width: 100%;
  position: relative;
}

</style>
