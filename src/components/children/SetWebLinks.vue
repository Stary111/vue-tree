<template>
  <section class="set-web-links">
    <aside>
      <el-form ref="form" :model="form" label-width="80px">
        <el-form-item label="菜单名称">
          <el-input v-model="form.menu_name" disabled></el-input>
        </el-form-item>
        <el-form-item label="序号">
          <el-input v-model="form.serial_number"></el-input>
        </el-form-item>
        <el-form-item label="菜单状态">
          <el-radio-group v-model="form.menu_status">
            <el-radio label="1">有效</el-radio>
            <el-radio label="0">无效</el-radio>
          </el-radio-group>
        </el-form-item>
        <el-form-item label="菜单url">
          <el-input type="textarea" v-model="form.menu_url"></el-input>
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
        menu_url: "",
      },
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
.set-web-links {
  display: flex;
  justify-content: center;
  margin-top: 20px;
}
.set-web-links aside {
  width: 400px;
}
</style>