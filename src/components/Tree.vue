<template>
  <div class="white-body-view">
    <el-tree
        id="my-tree"
        ref="tree"
        class="tree-view structure-tree"
        :data="treeData"
        highlight-current
        :default-expand-all="treeExpandAll"
        :props="defaultProps"
        check-strictly
        :node-key="treeNodeKey"
        :auto-expand-parent="false"
        :expand-on-click-node="false"
    >
      <span slot-scope="{ node, data }" class="custom-tree-node">
        <span class="tooltip">
          <span class="add-f-s-14">{{ data.name }}</span>
        </span>
        <div v-if="node.isCurrent === true" class="operation-view">
          <i
              class="small-operation-btn el-icon-plus"
              @click.stop="handleAdd(data)"
          />
          <i
              class="small-operation-btn el-icon-edit "
              @click.stop="handleEdit(data)"
          />
          <i
              class="small-operation-btn el-icon-delete"
              @click.stop="handleDelete(data)"
          />
        </div>
      </span>
    </el-tree>
  </div>
</template>

<script>
export default {
  props: {
    // 源数据
    treeData: {
      type: Array,
      default: function () {
        return []
      }
    },

    // 树节点是否默认展开
    treeExpandAll: {
      type: Boolean,
      default: true
    },

    // 树节点唯一标识
    treeNodeKey: {
      type: String,
      default: ''
    }
  },

  data() {
    return {
      defaultProps: {
        children: 'children',
        label: 'name'
      },
      selectItem: {}
    }
  },

  methods: {
    // 添加新增按钮
    handleAdd(data) {
      this.$emit('addItem', data)
    },

    // 点击删除按钮
    handleDelete(data) {
      this.$emit('deleteItem', data)
    },

    // 点击编辑按钮
    handleEdit(data) {
      this.selectItem = data
      this.$emit('editItem', JSON.parse(JSON.stringify(data)))
    },

    // ============== 组件内事件 结束=============

    // ============== 父组件回调事件 开始=============

    // 添加新记录，树形列表回显
    treeAddItem(data) {
      this.$refs.tree.append(data, data.parentId)
    },

    // 删除节点
    treeDeleteItem(val) {
      this.$refs.tree.remove(val)
    },

    // 修改记录，树形列表回显
    treeEditItem(val) {
      Object.assign(this.selectItem, val)
      this.selectItem = {}
    }

    // ============== 父组件回调事件 结束=============

  }
}
</script>
<style lang="less">
.structure-tree {
  .el-scrollbar .el-scrollbar__wrap {
    overflow-x: hidden;
  }

  #my-tree .el-tree > .el-tree-node {
    min-width: 100%;
    display: inline-block;
  }

  .el-tree-node__content {
    margin-bottom: 10px;
  }

  .tooltip {
    margin-right: 5px;
    font-size: 13px;
    border-radius: 4px;
    box-sizing: border-box;
    white-space: nowrap;
    padding: 4px;
  }

  .operation-view {
    display: inline-block;
    padding: 0 5px;
    margin-left: 5px;
    color: #777777;
  }

  .small-operation-btn {
    margin: 0 3px;
  }
}

</style>
