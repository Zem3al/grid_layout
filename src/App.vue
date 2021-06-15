<template>
  <div id="app">
    <div v-show="false">
    <ckeditor :editor="editor" v-model="editorData" :config="editorConfig"></ckeditor>
    </div>
    <div class="modal" v-show="modal" @click="closeModal">
      <div class="modal-holder" @click.prevent="">
        <h3> ADD AN ELEMENT TO WHAT EVER</h3>
        <div style="display: flex;align-items: center;justify-content: center">
        <button class="btnn">
          Widgets
        </button >
          <button class="btnn">
            Text
          </button>
          <button class="btnn">
            Photo
          </button>
        </div>
        <div class="modal-content">
        <div class="widget" v-if="tab == 1">
          <h3> Insert A Rectangle</h3>
          <div >WIDTH: <input v-model="width"> </div>
          <div>
            HEIGHT: <input v-model="heigh">
          </div>
          <div> <button @click="addObject"> Submit</button> </div>
        </div>
          <div class="text" v-if="tab == 2">
          </div>
          <div class="photo" v-if="tab == 3">
          </div>
        </div>
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
        :manual-pagination="true"
        pdf-format="a4"
        pdf-orientation="landscape"
        pdf-content-width="1000px"

        ref="html2Pdf"
    >
      <section slot="pdf-content" >
        <grid_test :layout.sync="layout" :content="content" v-on:turnOnmodal="turnOnModal">
        </grid_test>
      </section>
    </vue-html2pdf>
  </div>
</template>

<script>
import grid_test from "./components/grid_test";
import VueHtml2pdf from 'vue-html2pdf'
import ClassicEditor from '@ckeditor/ckeditor5-build-classic';

export default {
  name: 'App',
  components: {
    grid_test,
    VueHtml2pdf,
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
      editing : 0,
      editor: ClassicEditor,
      editorData: '<p>Content of the editor.</p>',
      editorConfig: {
        toolbar: [
          'heading', 'fontFamily', 'undo', 'redo'
        ]
      },
      content : [],
      tab: 1,
      width: 100,
      heigh: 200,
    }
  },
  watch: {
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
    print_layout() {
      this.printlayout =  !this.printlayout
    },
    turnOnModal(e) {
      this.modal =true
      this.editing = e
    },
    closeModal(e) {
      if (e.target.className === 'modal') {
        this.modal = false
      }
    },
    addObject() {
      const body = `<div style="width: ${this.width}px;height: ${this.heigh}px;background: #42b983">TESTING</div>`
      this.modal = false
      this.$set(this.content,this.editing,body)
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
  z-index: 1;
  overflow-x: hidden;
  overflow-y: hidden;
  width: 100vw;
  height: 100vh;
  background-color: rgba(0,0,0,0.4);
  position: fixed;
  top: 0;
  left: 0;
  justify-content: center;
  align-items: center;
}

.modal-holder {
  z-index: 2;
  background: white;
  height: 70vh;
  width: 70vw;
  opacity: 2;
  margin: 5% auto auto; /* 15% from the top and centered */
  border: 1px solid #888;
  display: flex;
  flex-direction: column;
}

.btnn {
  width: 30%;
  padding: 10px;
  text-decoration: none;
  background: none;
  display: flex;
  text-align: center;
  border: none;
  border-top: 1px solid black;
  justify-content: center;
}

.modal-content {
  height:100%;
  padding: 20px;
}

.widget {
  height: 100%;
  width: 100%;
  background-color: #42b983;
  display: flex;
  position: relative;
  flex-direction: column;
}


</style>
