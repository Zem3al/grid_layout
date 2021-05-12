<template>
  <div id="app">
    <input v-model="col">
    <button @click.prevent="add" > ADD</button>
    <grid :grids="grids"></grid>
  </div>
</template>

<script>
import grid from './components/grid.vue'

export default {
  name: 'App',
  components: {
    grid
  },
  data () {
    return {
      grids: [],
      col: 0,
    }
  },
  mounted() {
    this.grids.push({col: 3 })
  },
  methods: {
    add() {
      this.grids.push({col: parseInt(this.col)})
      console.log(this.grids)
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
}
</style>
