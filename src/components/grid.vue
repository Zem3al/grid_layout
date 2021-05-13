<template>
  <div>
    <div class="grid-container" :style="style(grid)"  v-for="(grid,i) in grids"  :key="'container ' + i" >
      <div class="grid-item"  v-for="col in grid.col" :key="'container '+ i + 'col ' + col">
          <slot name="a"></slot>
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
    slots: {
      type: Array,
      required: true
    }
  },
  data() {
    return {
     a : [],
    }
  },
  methods: {
    style(grid) {
        // eslint-disable-next-line no-unused-vars
       return [{ 'grid-template-columns' : Array.from({length: grid.col},  (_,i) => (i = 'auto ')).toString().replaceAll(',','')},{'grid-auto-rows': grid.size.toString().concat('px')}]
    },
  },
}
</script>

<style scoped>
.grid-container {
  display: grid;
  background: whitesmoke;
  padding: 10px;
  grid-column-gap: 50px;

}

.grid-item {
  border: 1px dashed rgba(0, 0, 0, 0.8);
  padding: 20px;
  font-size: 30px;
  text-align: center;
}
</style>
