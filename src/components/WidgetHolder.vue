<template>
  <div>
    <VueResizable>

    </VueResizable>
  </div>
</template>

<script>
import VueResizable from 'vue-resizable'
export default {

  name: "WidgetHolder",
  components: {
    VueResizable
  },
  props: {
    content: Object
  },

}
</script>

<style scoped>

</style>
