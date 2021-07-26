<template>
  <section class="container">
    <h1 class="title">
      This page is loaded from the {{ name }}
    </h1>
    <div id="viewDiv"></div>
  </section>
</template>
<script>
import { loadModules } from 'esri-loader'
export default {
  data ({ req }) {
    return {
      name: req ? 'server' : 'client'
    }
  },
  head () {
    return {
      title: `Map Page (${this.name}-side)`
    }
  },
  mounted () {  
    console.log('map: mounted')
    loadModules([
      'esri/config',
      'esri/Map',
      'esri/views/SceneView',
      'esri/core/watchUtils'
    ], {
      // use a specific version instead of latest 4.x
      url: 'https://js.arcgis.com/4.2/'
    }).then(([esriConfig, EsriMap, SceneView]) => {
      // create map with the given options at a DOM node w/ id 'mapNode'
      esriConfig.apiKey = process.env.API_KEY
      let map = new EsriMap({
          basemap: 'topo',
          id: '41281c51f9de45edaf1c8ed44bb10e30'
        })
       let view = new SceneView({
        container: 'viewDiv',
        center: [-118.80500,34.02700],
        zoom: 13,
        map
      })
      // NOTE: important: now that we're using a promise
      // your callback must NOT return any v4.x classes that resolve to promises
      // this will cause a hole in the space-time continum that will kill us all
      // return view
    })
 }
}
</script>

<style scoped>
@import url('https://js.arcgis.com/4.2/esri/css/main.css');
#viewDiv {
  height: 500px;
  width: 100%;
}
.title
{
  margin-top: 50px;
}
.info
{
  font-weight: 300;
  color: #9aabb1;
  margin: 0;
  margin-top: 10px;
}
.button
{
  margin-top: 50px;
}
</style>