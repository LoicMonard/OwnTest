<template>
  <div id="app">
    <div>
      <input id="searchTextField" type="text">
      <button @click="addMarker()">Get loc details</button>
      <button @click="getLocPhoto()">Get loc photo</button> {{address}}
      <img v-bind:src="this.response">
      <div id="myMap"></div>
      <li v-for="marker in markers">
        - <span class="message">{{marker.address}}</span><br>
        at lat : <span class="author">{{marker.latitude}}</span>
        at lng : <span class="date">{{marker.longitude}}</span>
      </li>
    </div>
  </div>
</template>

<script>
import Map from './Map.vue'
import Navbar from './Navbar.vue'
import $ from 'jquery'

export default {
  data () {
    return {
      address: '',
      response: '',
      markers: '',
      infoWindows: []
    }
  },
  methods: {
    getLoc() {
      this.address = document.getElementById('searchTextField').value;
      var xmlHttp = new XMLHttpRequest();
      xmlHttp.open("GET", "https://maps.googleapis.com/maps/api/geocode/json?address="+this.address+"&key=AIzaSyCUEEzTpu8BzuQ2te4yY0gBzvQ6ax7w_wA", false);
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

      var infoWindow = new google.maps.InfoWindow();

      var content = this.response.results[0].formatted_address + '<br><img src="'+imgUrl+'">';

      marker.addListener('click', function() {
        infoWindow.close();
        infoWindow.setContent(content);
        infoWindow.open(this.map, marker);
      });

      this.map.panTo(marker.getPosition());
      this.map.setZoom(14);

      marker.addListener('dblclick', function() {
        this.map.panTo(this.getPosition());
        this.map.setZoom(14);
      });
    }
  },
  mounted: function() {
    //MAP INIT
    this.map = new google.maps.Map(document.getElementById('myMap'), {
      center: {lat:48.709444, lng: 2.492176},
      scrollwheel: true,
      zoom: 14
    })
    //MARKER INIT
    var marker = new google.maps.Marker({
        map: this.map,
        position: new google.maps.LatLng(40.72, -74)
    });
    //INFOWINDOW INIT
    var infoWindow = new google.maps.InfoWindow;

    //AUTOCOMPLETE INIT
    var autocomplete;
    
    var input = document.getElementById('searchTextField');

    var options = {
      componentRestrictions: {'country':'fr'},
      types: [("locality", "political", "geocode")] // (cities)
    };

    autocomplete = new google.maps.places.Autocomplete(input,options);

    this.$http.get('https://api.myjson.com/bins/11dq3r').then(response => {
      this.markers = response.body;
      console.log(response);

      
      Array.prototype.forEach.call(this.markers, (marker) => {
        var id = marker.id;
        var title = marker.title;
        var address = marker.address;
        var point = new google.maps.LatLng(
          parseFloat(marker.latitude),
          parseFloat(marker.longitude)
        );

        var infowincontent = document.createElement('div');
        var strong = document.createElement('strong');
        strong.textContent = title;
        infowincontent.appendChild(strong);
        infowincontent.appendChild(document.createElement('br'));
        var text = document.createElement('text');
        text.textContent = address
        infowincontent.appendChild(text);

        var marker = new google.maps.Marker({
          position: point,
          map: this.map
        });

        marker.addListener('click', function() {
          infoWindow.setContent(infowincontent);
          infoWindow.open(this.map, marker);
        });

        marker.addListener('dblclick', function() {
          this.map.panTo(this.getPosition());
          this.map.setZoom(14);
        });

        console.log("Id : "+id+", title : "+ title + ", address : "+address+", point : "+point);
      });
      
    }, (response) => {
      console.log('erreur');
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
