<template>
  <div>
    <draggable v-model="cells" handle=".handle">
      <transition-group tag="div" class="grid-container" name="grid">
        <div
            v-for="(cell,i) in cells" :key="getID(cell,i)" class="grid-item"
            :style="style(cell)"
        >
          <div class="handle" />
          {{ cell.spancol }}
          <div class="grid-content">
            <div :class="ResizeAble(cell.row -1,cell.wid,true)?'resize':'resize_disable'" @click.prevent="resizeComponent(cell.row -1,cell.wid,true)" />
            <slot :name="i">
              <div v-if="content[cell.wid]">
                <div v-html="content[cell.wid]" />
              </div>
              <div v-else>
                <div class="circle" @click="insertItem(cell.wid)">
                  <div class="line roltage" />
                  <div class="line" />
                </div>
              </div>
            </slot>
            <div :class="ResizeAble(cell.row -1,cell.wid,false)?'resize':'resize_disable'" @click.prevent="resizeComponent(cell.row -1,cell.wid,false)" />
          </div>
        </div>
      </transition-group>
    </draggable>
  </div>
</template>
<script>
import draggable from 'vuedraggable'

export default {
  name: 'grid_test',
  components: {
    draggable
  },
  props: {
    layout: {
      type: Object,
      required: true
    },
    content: {
      type: Object,
      required: true
    }
  },
  computed: {
    cells: {
      get: function () {
        console.log('get')
        const cells = []
        this.layout.rows.forEach((grid, row) => {
          // eslint-disable-next-line no-unused-vars
          cells.push(...Array.from({ length: grid }, (v, i) => (v = {
            row: row + 1,
            column: this.getColumn(row, i),
            spancol: this.content[this.getWid(i, row)].span,
            spanrow: 1,
            wid: this.getWid(i, row)
          })))
        })
        return cells
      },
      set: function (newvalue) {
        const c = Array.from({ length: newvalue.length }, (v, i) => (v = newvalue[i]).wid)
        const newValue = this.layout
        newValue.wid = c
        this.updateLayout(newValue)
      }
    },
    rowMapping: function () {
      const mapping = []
      let currentIndex = 0
      this.layout.rows.forEach((row) => {
        mapping.push(this.layout.wid.slice(currentIndex, currentIndex + row))
        currentIndex += row
      })
      return mapping
    }
  },
  methods: {
    style (cell) {
      // eslint-disable-next-line no-unused-vars
      const a = [{ 'grid-area': ''.concat(...[cell.row, '/', cell.column, '/', 'span ' + cell.spanrow, '/', 'span ' + cell.spancol]).replaceAll(',', '') }]
      return a
    },
    getWid (col, row) {
      let wid = 0
      for (let i = 0; i < row; i++) {
        wid += this.layout.rows[i]
      }
      return this.layout.wid[wid + col]
    },
    updateLayout (newValue) {
      this.$emit('update:layout', newValue)
    },
    insertItem (id) {
      this.$emit('turnOnmodal', id)
    },
    resizeComponent (row, wid, isLeft) {
      if (this.ResizeAble(row, wid, isLeft)) {
        let i = 0
        this.rowMapping[row].forEach((Colwid, index) => {
          if (Colwid === wid) {
            i = index
          }
        })
        if (isLeft) {
          const Cwid = this.rowMapping[row][i - 1]
          this.content[Cwid].span = this.content[Cwid].span - 1
          this.content[wid].span = this.content[wid].span + 1
        }
        if (!isLeft) {
          const Cwid = this.rowMapping[row][i + 1]
          this.content[Cwid].span = this.content[Cwid].span - 1
          this.content[wid].span = this.content[wid].span + 1
        }

        this.$set(this, 'content', this.content)
      }
    },
    ResizeAble (row, wid, isLeft) {
      return this.rowMapping[row].reduce((resizeable, Colwid,index) => {
        if (Colwid === wid) {
          if(isLeft) {
            return (this.rowMapping[row][index-1]||this.rowMapping[row][index-1] == 0?(this.content[this.rowMapping[row][index-1]].span > 1 ?true:false):false)
          }
          return (this.rowMapping[row][index+1]?(this.content[this.rowMapping[row][index+1]].span > 1 ?true:false):false)
        }
        return resizeable
      }, false)
    },
    getColumn (row, col) {
      col = this.rowMapping[row].reduce((cols, colwid, colindex) => {
        if (colindex >= col) {
          return cols
        }
        return cols + this.content[colwid].span
      }, 1)
      return col
    },
    getSpan (wid) {
      return this.content[wid].span
    },
    getID (cell, i) {
      return `${i}${cell.wid}${this.content[cell.wid].span}`
    }
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
  grid-row-gap: 10px;
}

.grid-content {
  font-size: 30px;
  text-align: center;
  display: flex;
  justify-content: space-between;
  position: relative;
}

.grid-item {
  border: 1px dashed rgba(0, 0, 0, 0.8);
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

.resize{
  width: 10px;
  background-color: red;
}

.resize_disable {
  width: 10px;
  background-color: gray;
}

.roltage {
  transform: rotate(90deg);
}
.handle {
  height: 10px;
  background-color: #42b983;
}

</style>
