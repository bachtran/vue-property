<template>
  <div id="app">
    <Sidebar :filterData="featuresData" @filter="filter" class="sidebar"/>
    <Map :mapData="activeData" :accessToken="mapboxAccessToken" :mapStyle="mapboxMapStyle" class="map"/>
  </div>
</template>

<script>
import Sidebar from './components/Sidebar.vue'
import Map from './components/Map.vue'
import data from '@/data/testBlob.json'
export default {
  name: 'App',
  components: {
    Sidebar,
    Map,
  },
  data: function () {
    return {
      featuresData: data.features,
      activeData: data.features,
      mapboxAccessToken: "pk.eyJ1IjoiYmFjaHRyYW4iLCJhIjoiY2thN3N5ZjZ2MDdxYjJ0dGRyeHAxbHM4YiJ9.k85hRxXuCHMBcnuIDXVpIg",
      mapboxMapStyle: "mapbox://styles/mapbox/streets-v9",
    }
    
  },
  created: function () {
    console.log('App created. Loading data');
    console.log(data);
  },
  methods: {
    filter: function (e) {
      console.log("Filter called", e);
      this.activeData = e;
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
}
.sidebar {
    width: 20%;
}
.map {
  flex: 1;
  overflow: auto;
}
</style>
