<template>
  <div class="table-box">
    <p v-if="fixedHead" class="header-box">{{fixedHead}}</p>
    <el-table :data="data" border fit height="calc(100% - 40px)" style="width: 100%" :row-style="showTr" tooltip-effect="dark" class="tree-table">
      <el-table-column type="selection" width="40">
      </el-table-column>
      <el-table-column v-for="(column, index) in columns" :key="column.dataIndex" :label="column.text" :min-width="firstWidth(index)" show-overflow-tooltip :align="firstAlign(index)">
        <template slot-scope="scope">
          <span v-if="spaceIconShow(index)" v-for="(space, levelIndex) in scope.row._level" class="ms-tree-space" :key="levelIndex"></span>
          <span class="button" v-if="toggleIconShow(index,scope.row)" @click="toggle(scope.$index)">
                <i v-if="!scope.row._expanded" class="el-icon-remove" aria-hidden="true"></i>
                <i v-if="scope.row._expanded" class="el-icon-circle-plus" aria-hidden="true"></i>
              </span>
          <span v-else-if="index===0" class="ms-tree-space"></span>
          {{scope.row[column.dataIndex]}}
        </template>
      </el-table-column>
      <el-table-column label="操作" width="200" v-if="treeType === 'normal'">
        <template slot-scope="scope">
          <el-button type="primary" size="mini" @click="showEditDialog(scope.row)">编辑</el-button>
          <el-button type="primary" size="mini" @click="showHandleAdd(scope.row)">添加子类</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>
<script>
import MSDataTransfer from './dataTranslate.js'
export default {
  name: 'tree-grid',
  props: {
    treeStructure: {
      type: Boolean,
      default: true
    },
    columns: {
      type: Array,
      default: () => []
    },
    dataSource: {
      type: Array,
      default: () => []
    },
    treeType: {
      type: String,
      default: 'normal'
    },
    defaultExpandAll: {
      type: Boolean,
      default: true

    },
    fixedHead: {
      type: String,
      default: ''
    },
    fixed: {
      type: Boolean,
      default: true
    },

    btnList: {
      type: Array,
      default: () => ([])
    },
    operatWidth: {
      type: Number,
      default: 300
    }
  },
  data() {
    return {}
  },
  computed: {
    data() {
      if (this.treeStructure) {
        let data = MSDataTransfer.treeToArray(this.dataSource, null, null, this.defaultExpandAll)
        return data
      }
      return this.dataSource
    }
  },
  methods: {
    showEditDialog(row) {
      console.log(row)
      alert("自己交互")
    },
    showHandleAdd(row) {

      alert("自己交互")
        console.log(row)
    },
    // -----------------------
    showTr(rows, index) {
      let row = rows.row
      let show = (row._parent ? (row._parent._expanded && row._parent._show) : true)
      row._show = show
      return show ? '' : 'display:none;'
    },
    toggle(trIndex) {
      let record = this.data[trIndex]
      record._expanded = !record._expanded
    },
    spaceIconShow(index) {
      if (this.treeStructure && index === 0) {
        return true
      }
      return false
    },
    toggleIconShow(index, record) {
      if (this.treeStructure && index === 0 && record.children && record.children.length > 0) {
        return true
      }
      return false
    },
    firstWidth(index) {
      if (index === 0) {
        return '150'
      } else {
        return '100'
      }
    },
    firstAlign(index) {
      if (index === 0) {
        return 'left'
      } else {
        return 'center'
      }
    }
  }
}

</script>
<style scoped>
.table-box {
  width: 100%;
  height: 100%;
}

.header-box {
  padding: 8px 0;
  margin: 0;
  text-align: center;
  font-weight: bold;
}

.ms-tree-space {
  position: relative;
  top: 1px;
  display: inline-block;
  font-family: 'Glyphicons Halflings';
  font-style: normal;
  font-weight: 400;
  line-height: 1;
  width: 18px;
  height: 14px;
}

.ms-tree-space::before {
  content: ""
}

table td {
  line-height: 26px;
}

.button {
  font-size: 20px;
  cursor: pointer;
}

.firstWidth {
  min-width: 150px;
}
.tree-table .el-table__body-wrapper {
  overflow-y: auto;
}

.el-icon-circle-plus,
.el-icon-remove {
  color: #409EFF;
}
</style>

