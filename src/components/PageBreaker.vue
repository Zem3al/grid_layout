<template>
  <div style="position: relative">
  <div class="rectangle" ref="para">
    TESTING
    <div class="page_break" :style="style" @mousedown="enableDrag" @mouseup="disenableDrag" @mousemove="drag" >
    ----------------------------------- Page Break -------------------------------------
    </div>
  </div>

  </div>
</template>

<script>


export default {
  name: "PageBreaker",
  data: () => {
    return {
      dragable: false,
      top: 0
    }
  },
  computed: {
    style:  function () {
    return [{'top':`${this.top}px`}]
    }
  },
  methods: {
    enableDrag() {
      this.dragable = true
    },
    disenableDrag() {
      this.dragable = false
    },
    drag(e) {
      if(this.dragable) {
          console.log(e.layerX)
          this.top = e.layerX
      }
    }
  }
}
</script>

<style scoped>

.rectangle {
  height: 2000px;
  width: 800px;
  background-color: burlywood;
  border: 1px dashed black;
  position: relative;
}

.page_break {
  position: absolute;
  z-index: 1;
  background: rgba(0, 0, 0, 0);
  right: 70px;
  font-size: 24px;
  color: chartreuse;
  cursor: pointer;
}

</style>
