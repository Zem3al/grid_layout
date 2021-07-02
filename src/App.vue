<template>
  <div id="app">
    <div class="modal" v-show="modal" @click="closeModal">
      <div class="modal-holder" @click.prevent="">
        <h3> ADD AN ELEMENT TO WHAT EVER</h3>
        <div style="display: flex;align-items: center;justify-content: center">
        <button class="btnn" @click.prevent="changeTab(1)">
          Widgets
        </button >
          <button class="btnn" @click.prevent="changeTab(2)">
            Text
          </button>
          <button class="btnn" @click.prevent="changeTab(3)">
            Photo
          </button>
        </div>
        <div class="modal-content">
        <div class="widget" v-if="tab == 1">
        </div>
          <div class="text" v-if="tab == 2">
            <select v-model="widget">
              <option value="pie">Circle</option>
              <option value="line">Line Graph</option>
              <option value="table">Table</option>
            </select>
            <div style="display: flex; justify-content: space-between">
              <SVGLoader :type="widget" :height="200" :width="200"></SVGLoader>
              <div>
                <button @click.prevent="saveWiget()">Save</button>
                Time:
                Date:
              </div>
            </div>
          </div>
          <div  class="photo" v-if="tab == 3">
          </div>
        </div>
      </div>
    </div>
    COL: <input v-model="col">
    SIZE: <input v-model="size">
    <button @click.prevent="add"> ADD</button>
    <button @click.prevent="print_layout" class="btnfly"> See PrintLayout</button>
    <div style="max-width: 100vw">
      <grid_test :id="key" :layout.sync="reportLayout" :content.sync="content" @turnOnmodal="turnOnModal" />
    </div>
  </div>
</template>

<script>
import ClassicEditor from '@ckeditor/ckeditor5-editor-classic/src/classiceditor';
import SVGLoader from "./components/SVGLoader";
import EssentialsPlugin from '@ckeditor/ckeditor5-essentials/src/essentials';
import BoldPlugin from '@ckeditor/ckeditor5-basic-styles/src/bold';
import ItalicPlugin from '@ckeditor/ckeditor5-basic-styles/src/italic';
import LinkPlugin from '@ckeditor/ckeditor5-link/src/link';
import ParagraphPlugin from '@ckeditor/ckeditor5-paragraph/src/paragraph';


import grid_test from "./components/grid_test";

export default {
  name: 'App',
  watch: {
    content () {
      console.log('hallo')
    }
  },
  components: {
    grid_test,
    SVGLoader
  },
  computed: {},
  data() {
    return {
      col: 4,
      key: 0,
      size: 100,
      slots: [],
      widspan: {},
      reportLayout: {
        rows: [],
        wid: [],
      },
      content: {},
      widget: '',
      editor: ClassicEditor,
      editorData: '<p>Content of the editor.</p>',
      editorConfig: {
        plugins: [
          EssentialsPlugin,
          BoldPlugin,
          ItalicPlugin,
          LinkPlugin,
          ParagraphPlugin
        ],
        toolbar: {
          items: [
            'bold',
            'italic',
            'link',
            'undo',
            'redo'
          ]
        }
      },
      printlayout: false,
      modal : false,
      editing : 0,
      tab: 1,
    }
  },
  mounted() {
    this.layout.rows.push(3)
    this.layout.wid.push(0, 1, 2)
    this.lmao = ClassicEditor
  },
  methods: {
    lmaoxd(evt) {
      console.log(evt.plugins)
    },
    add () {
      this.reportLayout.rows.push(parseInt(this.col))
      Array.from({ length: this.col }, (v, i) => (v = this.reportLayout.wid.length + i)).forEach((wid) => {
        this.reportLayout.wid.push(wid)
        this.content[wid] = { span: 12 / (parseInt(this.col)) }
      })
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
    },
    changeTab(tab) {
      this.tab = tab
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
