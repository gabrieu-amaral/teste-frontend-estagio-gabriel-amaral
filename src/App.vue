<script >
import HelloWorld from './components/HelloWorld.vue'
import MyMap from './components/MyMap.vue'
import leaflet from "leaflet"
import { onMounted } from '@vue/runtime-core'
import Navbar from './components/Navbar.vue'

export default {
  name: 'App',
  components: {
    MyMap,
    Navbar,
    HelloWorld,
  
},

    setup() {
      let mymap;

      onMounted(() => {
        mymap = leaflet.map('mapid').setView([-19.126536, -45.947756], 13);

      leaflet
        .tileLayer(
          "https://api.mapbox.com/styles/v1/{id}/tiles/{z}/{x}/{y}?access_token=pk.eyJ1IjoiYWxwaGFkdXN0IiwiYSI6ImNsNHI0ZmE0cDBndzIzanM2a3MzdXB3ZjMifQ.ibD0f-ykeycI-_Z9__0rxw",
          {
            attribution:
              'Map data &copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors, Imagery © <a href="https://www.mapbox.com/">Mapbox</a>',
            maxZoom: 18,
            id: "mapbox/streets-v11",
            tileSize: 512,
            zoomOffset: -1,
            accessToken:
              "pk.eyJ1IjoiYWxwaGFkdXN0IiwiYSI6ImNsNHI0ZmE0cDBndzIzanM2a3MzdXB3ZjMifQ.ibD0f-ykeycI-_Z9__0rxw",
          }
        )
        .addTo(mymap);
    });
    },
  }

</script>


<template>
   <Navbar />
  <div class="pt-28 grid gridzada sm:grid-cols-1 md:grid-cols-1 gap-10 bg-green-50">
   
    <MyMap class="g2"/>
    <HelloWorld/>
  </div>
  
  
   
</template>

<style>
.gridzada {
      grid-template-columns: repeat(3, minmax(0, 1fr));
}
.g2{
  	grid-column: span 2 / span 2;
}
</style>
