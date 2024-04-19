<template>
  <div class="white-body-view">
    <el-button type="primary" @click="addNewRecord()">新增</el-button>

    <el-tree
        id="my-tree"
        ref="tree"
        class="tree-view structure-tree"
        :data="treeData"
        highlight-current
        :default-expand-all="true"
        :props="defaultProps"
        check-strictly
        :auto-expand-parent="false"
        :expand-on-click-node="false"
    >
      <span slot-scope="{ node, data }" class="custom-tree-node" @mouseenter="handleMouseEnter(data)"
            @mouseleave="handleMouseLeave(data)">
        <span class="tooltip">
          <span class="add-f-s-14">{{ data.name }}</span>
        </span>
        <div v-show="data.show || node.isCurrent" class="operation-view">
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
    <place-dialog ref="placeDialog" @addData="addData" @editData="editData"/>
  </div>
</template>

<script>
import PlaceDialog from "@/components/PlaceDialog.vue";

export default {
  components: {
    PlaceDialog
  },
  data() {
    return {
      treeData: [
        {
          children: [
            {
              children: [],
              name: '1楼',
              desc: '这是教学楼1楼',
              parentId: '1',
              id: '2'
            },
            {
              children: [],
              name: '2楼',
              desc: '这是教学楼1楼',
              parentId: '1',
              id: '3'
            },
            {
              children: [],
              name: '3楼',
              desc: '这是教学楼3楼',
              parentId: '1',
              id: '4'
            }
          ],
          name: '教学楼',
          parentId: '',
          id: '1'
        },
        {
          children: [
            {
              children: [],
              name: '办公1楼',
              desc: '这是办公楼',
              parentId: '5',
              id: '6'
            }
          ],
          name: '办公楼',
          parentId: '',
          id: '5'
        }
      ],
      defaultProps: {
        children: 'children',
        label: 'name'
      },
      selectItem: {},
      showOperationView: false
    }
  },

  methods: {
    // 添加新记录
    addNewRecord() {
      this.$refs.placeDialog.openDialog(false)
    },
    // 添加新增按钮
    handleAdd(data) {
      this.$refs.placeDialog.openDialog(false, data.id)
    },

    // 点击删除按钮
    handleDelete(data) {
      this.$confirm('确定删除吗?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        // 删除树节点
        this.treeDeleteItem(data)
        // 提示
        this.$message({
          type: 'success',
          message: '删除成功!'
        })
      }).catch(() => {
        this.$message({
          type: 'info',
          message: '已取消删除'
        })
      })
    },

    // 点击编辑按钮
    handleEdit(data) {
      this.selectItem = data
      this.$refs.placeDialog.openDialog(true, data.parentId, data)
    },

    // ============== 组件内事件 结束=============

    // ============== 父组件回调事件 开始=============

    // 删除节点
    treeDeleteItem(val) {
      this.$refs.tree.remove(val)
    },

    // ============== 父组件回调事件 结束=============

    handleMouseEnter(data) {
      this.$set(data, 'show', true)
    },

    handleMouseLeave(data) {
      this.$set(data, 'show', false)
    },

    // 新增表单数据
    addData(data) {
      // 新增树节点
      this.$refs.tree.append(data, data.parentId)
    },

    // 修改表单数据
    editData(data) {
      // 修改树节点
      Object.assign(this.selectItem, data)
      this.selectItem = {}
    },
  }
}
</script>

<style lang="less">
.white-body-view {
  width: 300px;

  .structure-tree {
    display: flex;
    flex-direction: column;

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

    .custom-tree-node {
      justify-content: space-between;
      display: flex;
      width: 100%;
    }
  }
}


</style>
