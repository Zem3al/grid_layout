<template>
  <div>
    <div class="grid-container" :style="style(grid.col)"  v-for="(grid,i) in grids"  :key="i" >
      <div class="grid-item"   v-for="i in grid.col" :key="i">
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "grid",
  props: {
    grids: {
      type: Array,
      required: true
    },
  },
  data() {
    return {
     a : []
    }
  },
  methods: {
    style(col) {
        // eslint-disable-next-line no-unused-vars
       return { 'grid-template-columns' : Array.from({length: col},  (_,i) => (i = 'auto ')).toString().replaceAll(',','')}
    }
  },
}
</script>

<style scoped>
.grid-container {
  display: grid;
  background: whitesmoke;
  padding: 10px;
}

.grid-item {
  border: 1px dashed rgba(0, 0, 0, 0.8);
  padding: 20px;
  font-size: 30px;
  text-align: center;
}
</style>
