<template>
  <div class="app">
    <div id="myMap"></div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }},
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
  }

}
</script>

<style scoped>
body, html {
  height: 100%;
  width: 100%;
  margin: 0px;
  padding: 0px;
}
  
#myMap {
  height:100vh;
  width: 100%;
  position: relative;
}
</style>