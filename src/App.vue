<template>
  <div id="app">
    <div class="modal" v-show="modal">
      <div class="control-panel">
        <div> </div>
        <h3> ADD AN ELEMENT TO WHAT EVER</h3>
      </div>
    </div>
    COL: <input v-model="col">
    SIZE: <input v-model="size">
    <button @click.prevent="add"> ADD</button>
    <button @click.prevent="print_layout" class="btnfly"> See PrintLayout</button>
    <vue-html2pdf
        class="vuehtml"
        :show-layout="true"
        :float-layout="false"
        :enable-download="true"
        :preview-modal="true"
        :paginate-elements-by-height="1400"
        filename="hee hee"
        :pdf-quality="2"
        :manual-pagination="false"
        pdf-format="a4"
        pdf-orientation="landscape"
        pdf-content-width="1000px"

        ref="html2Pdf"
    >
      <section slot="pdf-content" >
        <grid_test :layout.sync="layout">
        </grid_test>
      </section>
    </vue-html2pdf>
    <button @click.prevent="alo"> Click Me!</button>
  </div>
</template>

<script>
import grid_test from "./components/grid_test";
import VueHtml2pdf from 'vue-html2pdf'

export default {
  name: 'App',
  components: {
    grid_test,
    VueHtml2pdf
  },
  computed: {},
  data() {
    return {
      col: 0,
      size: 100,
      slots: [],
      layout: {
        rows: [],
        wid: [],
      },
      printlayout: false,
      modal : false,
    }
  },
  watch: {
    layout: {
      deep: true,
      handler: () => {
      }

    }
  },
  mounted() {
    this.layout.rows.push(3)
    this.layout.wid.push(0, 1, 2)
  },
  methods: {
    add() {
      this.layout.rows.push(parseInt(this.col))
      this.layout.wid.push(...Array.from({length: this.col}, (v, i) => (v = this.layout.wid.length + i)))
    },
    alo() {
    },
    print_layout() {
      this.printlayout =  !this.printlayout
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

.btnfly {
  z-index: 3;
  position: absolute;
  right: 50px;
  background: #42b983;
}

.modal {
  z-index: 90;
  overflow-x: hidden;
  overflow-y: hidden;
  width: 100vw;
  height: 100vh;
  background: black;
  opacity: 0.5;
  position: absolute;
  top: 0;
  left: 0;
}

</style>
