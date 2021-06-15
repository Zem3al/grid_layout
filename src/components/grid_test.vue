<template>
  <div>
        <draggable v-model="cells">
          <transition-group tag="div" class="grid-container" name="grid">
            <div class="grid-item" v-for="(cell,i) in cells" :key="`${i}${cell.wid}`" :style="style(cell)">
              <slot :name=i>
                <div v-if="content[cell.wid]">
                  <div v-html="content[cell.wid]"> </div>
                </div>
                <div v-else>
                <div class="circle" @click="insertItem(cell.wid)">
                <div class="line roltage"> </div>
                <div class="line"> </div>
                </div>
              </div></slot>
            </div>
          </transition-group>
        </draggable>
  </div>
</template>
<script>
import draggable from 'vuedraggable'


export default {
  name: "grid_test",
  components: {
    draggable,
  },
  props: {
    layout: {
      type: Object,
      required: true
    },
    content: {
      type: Array,
      required: true
    }
  },
  computed: {
    cells: {
      get: function () {
        let cells = []
        this.layout.rows.forEach((grid, row) => {
          // eslint-disable-next-line no-unused-vars
          cells.push(...Array.from({length: grid}, (v, i) => (v = {
            row: row + 1,
            column: 1 + ((12 / grid) * i),
            spancol: (12 / grid),
            spanrow: 1,
            wid: this.getWid(i, row)
          })))
        })
        return cells
      },
      set: function (newvalue) {
        const c = Array.from({length: newvalue.length}, (v, i) => (v = newvalue[i]).wid)
        let newValue = this.layout
        newValue.wid = c
        this.updateLayout(newValue)
      }
    }
  },
  data() {
    return {
      a: [],
    }
  },
  methods: {
    style(cell) {
// eslint-disable-next-line no-unused-vars
      let a = [{'grid-area': ''.concat(...[cell.row, '/', cell.column, '/', 'span ' + cell.spanrow, '/', 'span ' + cell.spancol]).replaceAll(',', '')}]
      return a
    },
    getWid(col, row) {
      let wid = 0
      for (let i = 0; i < row; i++) {
        wid += this.layout.rows[i]
      }
      return this.layout.wid[wid + col]
    },
    updateLayout(newValue) {
      this.$emit('update:layout', newValue)
    },
    updateBreakPage(component) {
      const GridContainers = document.getElementsByClassName('grid-container')
      for(let i = 0 ;i <component ;i ++) {
        console.log(GridContainers[i])
      }
    },
    LogInfomation() {
      console.log(this.layout)
    },
    insertItem(id) {
      this.$emit('turnOnmodal',id)
    },
  },

}
</script>

<style scoped>
.grid-container {
  display: grid;
  background: whitesmoke;
  padding: 10px;
  grid-column-gap: 20px;
  grid-template-columns:  auto auto auto auto auto auto auto auto auto auto auto auto;
  grid-column-gap: 20px;
  grid-row-gap: 50px;
}

.grid-item {
  border: 1px dashed rgba(0, 0, 0, 0.8);
  padding: 10px;
  font-size: 30px;
  text-align: center;
  display: flex;
  justify-content: center;
}

.circle {
  border-radius: 50%;
  height: 40px;
  width: 40px;
  position: relative;
}

.circle:hover {
  background: white;
  opacity: 1;
  cursor: pointer;
}

.line {
  width: 20px;
  height: 2px;
  background: black;
  position: absolute;
  top: 47%;
  right: 24%;
}

.roltage {
  transform: rotate(90deg);
}
</style>
