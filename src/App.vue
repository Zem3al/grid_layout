<template>
  <div id="app">
    COL: <input v-model="col">
    SIZE: <input v-model="size">
    <button @click.prevent="add" > ADD</button>
    <grid ref="grid" :grids="grids" :slots.sync="slots"></grid>
    <button @click.prevent="alo"> Click Me!</button>
  </div>
</template>

<script>
import grid from './components/grid.vue'

export default {
  name: 'App',
  components: {
    grid
  },
  computed:{
    keys() {
      console.log(this.$refs.grid)
      return 1
    }
  },
  data () {
    return {
      grids: [],
      col: 0,
      size: 100,
      slots: [],
    }
  },
  mounted() {
    this.grids.push({col: 3, size:100 })
    this.slots.push(
        ...Array.from({ length: 3 }, (v, i) => (v = { in: this.slots.length + i, use: false }))
    );
  },
  methods: {
    add() {
      this.grids.push({col: parseInt(this.col), size: parseInt(this.size)})
      this.slots.push(
          ...Array.from({ length: this.col }, (v, i) => (v = { in: this.slots.length + i, use: false }))
      );
    },
    alo() {
      console.log(this.$refs.grid)
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
