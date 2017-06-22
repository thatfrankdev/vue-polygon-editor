<template>
  <div id="app">
    <textarea type="text" v-model="points" />
    <polygon-editor 
      v-model="points" 
      height="300px" 
      width="300px"
      stroke="black"
      fill="blue"
      background-image="https://unsplash.it/300/300/?random">
      </polygon-editor>
  </div>
</template>

<script>

import PolygonEditor from '../PolygonEditor.vue'

export default {
  name: 'app',
  components:{
    PolygonEditor
  },
  data () {
    return {
      points : "200,10 250,190 160,210"
    }
  }
}

</script>

<style lang="scss">

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

</style>
