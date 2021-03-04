<template>
  <!-- 新增编辑 弹窗 -->
  <div>
    <el-dialog
      :before-close="hideDialog"
      :title="title"
      :visible.sync="dialogVisible"
      width="30%"
    >
      <div>
        <el-form ref="form" :model="form" label-width="80px">
          <el-form-item label="活动名称">
            <el-input v-model="form.menu_name"></el-input>
          </el-form-item>
        </el-form>
      </div>
      <span slot="footer" class="dialog-footer">
        <el-button @click="hideDialog">取 消</el-button>
        <el-button type="primary" @click="save('form')">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
export default {
  data() {
    return {
      form: {
        menu_name: "",
      },
    };
  },
  props: {
    dialogVisible: {
      type: Boolean,
      default: false,
    },
    title: {
      type: String,
      default: "新增菜单",
    },
    edit_data: {
      type: Object,
      default: undefined,
    },
    type: {
      type: Number,
      default: 0,
    },
  },
  methods: {
    hideDialog() {
      this.$emit("fromChildCanael");
    },
    save(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          this.$emit("fromChildSave", {
            type: this.type,
            data: { ...this.edit_data, ...this.form },
          });
        } else {
          return false;
        }
      });
    },
  },
  watch: {
    dialogVisible(newValue, oldValue) {
      if (newValue) {
        this.form = { menu_name: "" };
      }
    },
    edit_data(newValue, oldValue) {
      this.form.menu_name = newValue.label;
      //   console.log(newValue, newValue.label, this.form);
    },
  },
};
</script>

<style >
</style>