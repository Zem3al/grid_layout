<template>
  <div >
    <div v-if="type=='pie'">
      <img :style="style" src="@/assets/chart/pie-chart.svg">
    </div>
    <div v-if="type=='line'">
      <img :style="style" src="@/assets/chart/line-chart.svg">
    </div>
    <div v-if="type=='table'">
      <img :style="style" src="@/assets/chart/table.svg">
    </div>
  </div>
</template>

<script>
// import Chart from './Chart'

export default {
  name: 'SvgLoader',
  props: {
    type: String,
    height: Number,
    width: Number
  },
  computed: {
    style () {
      return [{ height: `${this.height}px` }, { width: `${this.width}px` }]
    }
  },
  methods: {
    // checkType () {
    //   Chart().forEach(chart => {
    //     if (chart.value === this.type) {
    //       return true
    //     }
    //   })
    //   return false
    // },
    pie () {
      return (this.type === 'pie')
    },
    bar () {
      return (this.type === 'bar')
    },
    line () {
      return (this.type === 'line')
    },
    table () {
      return (this.type === 'table')
    }
  }
}
</script>

<style scoped>

</style>
