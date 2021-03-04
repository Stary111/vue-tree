<template>
  <section class="set-textarea">
    <aside>
      <el-form ref="form" :model="form" label-width="80px">
        <el-form-item label="菜单名称">
          <el-input v-model="form.menu_name" disabled></el-input>
        </el-form-item>
        <el-form-item label="序号">
          <el-input v-model="form.serial_number"></el-input>
        </el-form-item>
        <el-form-item label="菜单状态" class="menu-status">
          <el-radio-group v-model="form.menu_status">
            <el-radio label="1">有效</el-radio>
            <el-radio label="0">无效</el-radio>
          </el-radio-group>
        </el-form-item>
        <el-form-item label="选择模板">
          <el-select v-model="form.template" placeholder="请选择模板">
            <el-option
              v-for="(item, key) in template_data"
              :key="key"
              :value="item.value"
              >{{ item.label }}</el-option
            >
          </el-select>
        </el-form-item>
        <el-form-item label="菜单key">
          <el-input
            v-model="form.menu_key"
            placeholder="请以 m_ 开头"
          ></el-input>
        </el-form-item>
        <el-form-item label="文本内容">
          <el-input type="textarea" v-model="form.menu_content"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="onSubmit">保存</el-button>
        </el-form-item>
      </el-form>
    </aside>
  </section>
</template>

<script>
export default {
  data() {
    return {
      form: {
        menu_name: "",
        menu_status: 1,
        serial_number: "",
        menu_content: "",
        template: "",
        menu_key: "",
      },
      template_data: [
        {
          label: "模板1",
          value: 1,
        },
        {
          label: "模板2",
          value: 2,
        },
      ], // 下拉数据
    };
  },
  props: {
    info_data: {
      type: Object,
      default: undefined,
    },
  },
  methods: {
    onSubmit() {
      // 保持一级菜单接口
      this.$message({
        type: "success",
        message: "保持成功",
      });
    },
  },
  watch: {
    info_data(newValue) {
      this.form = newValue;
    },
  },
};
</script>

<style  scoped>
.set-textarea {
  display: flex;
  justify-content: center;
  margin-top: 20px;
}
.set-textarea aside {
  width: 400px;
}
.el-select {
  width: 100%;
}
.menu-status {
  display: flex;
  align-items: center;
  justify-content: flex-start;
}
.menu-status > .el-form-item__content {
  display: flex;
  margin-left: 0 !important;
}
</style>