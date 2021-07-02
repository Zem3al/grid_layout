<template>
  <div style="position: relative">
    <div>
      {{this.now}}
      {{this.mouse}}
      <button @click.prevent="addBreaker"> Add Page Breaker</button>
    </div>
  <div class="rectangle" ref="para">
    TESTING
    <div v-for="(pa,id) in pageBreakerPos" :key="pa.name">
    <div class="page_break" ref="page" :style="style" @mousemove="drag($event,id,pa.name)" @mousedown="enableDrag($event,pa.name)" @mouseup="disenableDrag(pa.name)" >
    ----------------------------------- Page Break -------------------------------------
    </div>
    </div>
  </div>

  </div>
</template>

<script>



export default {
  name: "PageBreaker",
  data: () => {
    return {
      dragable: new Object(),
      top: 0,
      mouse : 0,
      pageBreakerPos: [],
      now: 0
    }
  },
  computed: {
    style:  function () {
    return [{'top':`${this.top}px`}]
    }
  },
  methods: {
    enableDrag(e,name) {
      this.dragable[name] = true
      this.mouse = e.clientY
    },
    disenableDrag(name) {
      this.dragable[name] = false
    },
    drag(e,id,name) {
      console.log(e,id,name)
      if(e.buttons == 0) {
        this.dragable[name] = false
      }
      if(this.dragable[name]) {
        e.preventDefault();
        this.now = this.mouse - e.clientY
        this.mouse = e.clientY
        this.$refs.page[id].style.top = (this.$refs.page[id].offsetTop - this.now) + "px";
      }
    },
    addBreaker() {
      this.pageBreakerPos.push({name: `page_breaker${this.pageBreakerPos.length}`})
    }
  },
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
  font-size: 40px;
  color: chartreuse;
  cursor: pointer;
  user-select:none;
}

</style>
