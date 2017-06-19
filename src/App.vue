<template>
  <gmap-map id="gmap"
    :center="center"
    :zoom="2"
    style="width: 500px; height: 300px">
    <gmap-marker
      :key="index"
      v-for="(m, index) in markers"
      :position="m.position"
      :clickable="true"
      :draggable="true"
      @click="center=m.position"><gmap-info-window :opened="true" @click="opened=false">Hello world!</gmap-info-window></gmap-marker>
  </gmap-map>
</template>

<script>
import * as VueGoogleMaps from 'vue2-google-maps';
import Vue from 'vue';

Vue.use(VueGoogleMaps, {
  load: {
    key: 'AIzaSyD6dLezojKvoPusQzZO_8U9L7fohmVcR7M',
    v: 'OPTIONAL VERSION NUMBER',
    libraries: 'places', //// If you need to use place input
  }
});

export default {
  data () {
    return {
      center: {lat: 10.0, lng: 10.0},
      markers: [{
        position: {lat: 10.0, lng: 10.0}
      }, {
        position: {lat: 11.0, lng: 11.0}
      }]
    }
  }
}
</script>

<style>

body {
  padding: 0px;
  margin: 0px;
}

.vue-map {
  height: 500px;
}

#test {
  background: red;
}

</style>
