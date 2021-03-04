<template>
  <div class="custom-tree-container">
    <div class="tree-left-content">
      <div>
        <div class="operation-btn">
          <el-button
            size="mini"
            type="primary"
            icon="el-icon-plus"
            @click="addOneMenu"
            >添加菜单</el-button
          >
          <el-button size="mini" icon="el-icon-s-order">生成菜单</el-button>
        </div>
        <el-tree
          style="margin-top: 15px"
          :data="data"
          node-key="id"
          default-expand-all
          :props="defaultProps"
          :expand-on-click-node="false"
          @node-click="handleNodeClick"
        >
          <span class="custom-tree-node" slot-scope="{ node, data }">
            <span>{{ node.label }}</span>
            <span style="z-index: 9999">
              <el-button
                v-show="data.menu_level === 1"
                type="text"
                size="mini"
                @click="(e) => append(e, data)"
              >
                新增
              </el-button>
              <el-button type="text" size="mini" @click="(e) => edit(e, data)">
                编辑
              </el-button>
              <el-button
                type="text"
                size="mini"
                @click="(e) => remove(e, node, data)"
              >
                删除
              </el-button>
            </span>
          </span>
        </el-tree>
      </div>
    </div>
    <div class="tree-right-content">
      <RightForm :row_data="edit_data" />
    </div>
    <AddMenu
      v-show="dialogVisible"
      :dialogVisible="dialogVisible"
      :edit_data="edit_data"
      :type="type"
      @fromChildCanael="hideDialog"
      @fromChildSave="saveData"
    />
  </div>
</template>

<script>
let id = 1000;
import AddMenu from "@/components/AddMenu.vue";
import RightForm from "@/components/RightForm.vue";
export default {
  data() {
    const data = [
      {
        id: 1,
        menu_name: "一级 1",
        menu_level: 1, // 菜单等级
        serial_number: 1, // 序号
        menu_status: "1", // 是否有效
        menu_type: 0,
        children: [
          {
            id: 4,
            menu_name: "二级-未设置",
            menu_level: 2,
            menu_type: 0, // 菜单是否有设置数据 用于显示 不同的表单信息 0=>没有设置 1=>发送文本信息 2=>跳转到网页  3=>发送图文信息 4=>跳转小程序
            menu_url: "",
          },
        ],
      },
      {
        id: 2,
        menu_name: "一级 2",
        menu_level: 1,
        serial_number: 2,
        menu_status: "1",
        menu_type: 1,
        children: [
          {
            id: 5,
            menu_name: "二级 设置文本",
            menu_level: 2,
            menu_type: 1,
            menu_url: "",
          },
          {
            id: 6,
            menu_name: "二级 设置网页链接",
            menu_level: 2,
            menu_type: 2,
            menu_url: "www.baidu.com",
          },
        ],
      },
      {
        id: 3,
        menu_name: "一级 3",
        menu_level: 1,
        serial_number: 3,
        menu_status: "0",
        menu_type: 2,
        children: [
          {
            id: 7,
            menu_name: "设置图文",
            menu_level: 2,
            menu_type: 3,
            menu_url: "",
          },
          {
            id: 8,
            menu_name: "设置小程序",
            menu_level: 2,
            menu_type: 4,
            menu_url: "",
          },
        ],
      },
    ];
    return {
      data: JSON.parse(JSON.stringify(data)),
      dialogVisible: false,
      edit_data: {},
      type: 0, // 1==>新增一级菜单 2==>新增2级菜单 3==> 编辑
      defaultProps: {
        // 设置渲染字段别名
        children: "children",
        label: "menu_name",
      },
    };
  },

  methods: {
    // 新增二级菜单
    append(e, data) {
      e.stopPropagation();
      // const newChild = { id: id++, menu_name: `二级${id++}`, children: [] };
      // if (!data.children) {
      //   this.$set(data, "children", []);
      // }
      // data.children.push(newChild);
      console.log(data);
      this.type = 2;
      this.edit_data = data;
      this.dialogVisible = true;
    },
    // 编辑
    edit(e, data) {
      e.stopPropagation();
      this.edit_data = data;
      this.type = 3;
      this.dialogVisible = true;
    },
    // 删除
    remove(e, node, data) {
      e.stopPropagation();
      this.$confirm("此操作将永久删除此菜单项, 是否继续?", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning",
      }).then(() => {
        // 调用删除接口
        this.$message({
          type: "success",
          message: "删除成功!",
        });
      });
      // const parent = node.parent;
      // const children = parent.data.children || parent.data;
      // const index = children.findIndex((d) => d.id === data.id);
      // children.splice(index, 1);
    },
    /**
     * (data为点击该行的数据，用于右边数据展示)
     */
    handleNodeClick(data) {
      this.edit_data = data;
    },
    // 添加一级菜单
    addOneMenu() {
      this.type = 1;
      this.dialogVisible = true;
    },
    // 隐藏弹窗
    hideDialog() {
      this.type = 0;
      this.dialogVisible = false;
    },
    /**
     * type 操作类型
     *  data  请求参数
     */
    saveData({ type, data }) {
      console.log(data);
      if (type === 1) {
        // 调用新增一级菜单的接口
        this.clear();
      }
      if (type === 2) {
        // 新增二级目录
        this.clear();
        // 调用新增二级菜单的接口

        // const newChild = { id: id++, menu_name: data.menu_name, children: [] };
        // if (!data.children) {
        //   this.$set(data, "children", []);
        // }
        // data.children.push(newChild);
      }
      if (type === 3) {
        // 编辑
        this.clear();
        // 调用编辑接口
      }
    },
    clear() {
      this.type = 0;
      this.dialogVisible = false;
    },
  },
  components: {
    AddMenu,
    RightForm,
  },
};
</script>

<style >
body {
  margin: 0;
}
.custom-tree-container {
  display: flex;
  min-height: 100vh;
  overflow: hidden;
}
.tree-left-content {
  width: 300px;
  border: 1px solid #eee;
  margin-left: 20px;
  min-height: 100%;
}
.tree-left-content .operation-btn {
  padding: 15px 0px;
  border-bottom: 1px solid #eee;
}
.tree-right-content {
  width: calc(100% - 300px);
  display: flex;
  justify-content: center;
  padding-top: 15px;
}
.custom-tree-node {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-size: 14px;
  padding-right: 8px;
  width: 300px;
}
</style>>