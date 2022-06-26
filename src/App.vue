<script >
import {ref} from 'vue'
import HelloWorld from './components/HelloWorld.vue'
import MyMap from './components/MyMap.vue'
import leaflet, { map, marker } from "leaflet"
import { onMounted } from '@vue/runtime-core'
import Navbar from './components/Navbar.vue'
import equipment from '../data/equipment.json'
import equipmentPositionHistory from '../data/equipmentPositionHistory.json'
import equipmentState from '../data/equipmentState.json'
import equipmentStateHistory from '../data/equipmentStateHistory.json'

export default {
  name: 'App',
  components: {
    MyMap,
    Navbar,
    HelloWorld,
  
},


    setup() {
      

      const selected = ref('')
      let mymap;

      onMounted(() => {
        mymap = leaflet.map('mapid').setView([-19.126536, -45.947756], 8);
        mymap.setZoom(10)
    


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

    var equipament = leaflet.icon({
      iconUrl: "../public/equipament-ok.png",
      shadowUrl: '../public/equipament-shadow.png',

      iconSize:     [40, 40], // size of the icon
      shadowSize:   [40, 40], // size of the shadow
      iconAnchor:   [22, 94], // point of the icon which will correspond to marker's location
      shadowAnchor: [10, 97],  // the same for the shadow
      popupAnchor:  [-3, -76] // point from which the popup should open relative to the iconAnchor
    });


    equipment.forEach(element => {
      var state = equipmentState.find((current) => current.id == equipmentStateHistory.find((state) => state.equipmentId == element.id).states[0].equipmentStateId).name
      var icon = null
      switch(state) {
        case "Parado":
          icon = leaflet.icon({
            iconUrl: "../public/equipament-stop.png",
            shadowUrl: '../public/equipament-shadow.png',

            iconSize:     [40, 40], // size of the icon
            shadowSize:   [40, 40], // size of the shadow
            iconAnchor:   [22, 94], // point of the icon which will correspond to marker's location
            shadowAnchor: [10, 97],  // the same for the shadow
            popupAnchor:  [-3, -76] // point from which the popup should open relative to the iconAnchor
          });

            var descrition = "Este equipamento está parado no momento"
          break

        case "Operando":
          icon = leaflet.icon({
            iconUrl: "../public/equipament-ok.png",
            shadowUrl: '../public/equipament-shadow.png',

            iconSize:     [40, 40], // size of the icon
            shadowSize:   [40, 40], // size of the shadow
            iconAnchor:   [22, 94], // point of the icon which will correspond to marker's location
            shadowAnchor: [10, 97],  // the same for the shadow
            popupAnchor:  [-3, -76] // point from which the popup should open relative to the iconAnchor
          });

          var descrition = "Este equipamento está operando"
          break

        case "Manutenção":
          icon = leaflet.icon({
            iconUrl: "../public/equipament-maintenance.png",
            shadowUrl: '../public/equipament-shadow.png',

            iconSize:     [40, 40], // size of the icon
            shadowSize:   [40, 40], // size of the shadow
            iconAnchor:   [22, 94], // point of the icon which will correspond to marker's location
            shadowAnchor: [10, 97],  // the same for the shadow
            popupAnchor:  [-3, -76] // point from which the popup should open relative to the iconAnchor
          });

          var descrition = "Este equipamento está em manutenção"
          break

      }
      leaflet.marker([equipmentPositionHistory.find((current) => element.id == current.equipmentId).positions[0].lat, equipmentPositionHistory.find((current) => element.id == current.equipmentId).positions[0].lon], {icon: icon}).addTo(mymap).on('click', () => selected.value = element.id)
        .bindPopup(descrition)
        .on('mouseover', function (e) {
            this.openPopup();
        })
        .on('mouseout', function (e) {
            this.closePopup();
        });
    });
  

    });
    return {selected}
  }
  
}



</script>


<template>
   <Navbar />

  <div class="w-screen h-screen pt-28 grid lg:grid-cols-3 sm:grid-cols-1 md:grid-cols-1 gap-10 bg-[url('../background.png')]  bg-slate-900 overflow-hidden">
   
    <MyMap class="col-span-2"/>
    <HelloWorld :selected="selected"/>
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
