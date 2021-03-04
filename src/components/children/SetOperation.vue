2<template>
  <!-- 设置动作 -->
  <section class="set-operation">
    <section v-show="info_data && menu_type === 0">
      <p>请选择订阅者点击菜单后，公众号做出的相应操作</p>
      <aside>
        <el-button type="primary" size="medium" @click="skipFormPage(1)"
          >发送文本信息</el-button
        >
        <el-button type="primary" size="medium" @click="skipFormPage(2)"
          >跳转到网页</el-button
        >
        <el-button type="primary" size="medium" @click="skipFormPage(3)"
          >发送图文信息</el-button
        >
        <el-button type="primary" size="medium" @click="skipFormPage(4)"
          >跳转小程序</el-button
        >
      </aside>
    </section>
    <SetWebLinks v-show="info_data && menu_type === 2" />
    <SetTextarea v-show="info_data && menu_type === 1" />
  </section>
</template>

<script>
import SetWebLinks from "@/components/children/SetWebLinks.vue";
import SetTextarea from "@/components/children/SetTextarea.vue";

export default {
  data() {
    return {
      menu_type: null, // 根据菜单类型显示不同的表单
    };
  },
  props: {
    info_data: {
      type: Object,
      default: undefined,
    },
  },
  mounted() {
    if (this.info_data) {
      this.menu_type = this.info_data.menu_type;
    }
  },
  methods: {
    skipFormPage(type) {
      this.menu_type = type;
    },
  },
  components: {
    SetWebLinks,
    SetTextarea,
  },
  watch: {
    info_data(newValue) {
      console.log(newValue);
      this.menu_type = newValue.menu_type;
    },
  },
};
</script>

<style scoped>
.set-operation {
  display: flex;
  flex-direction: column;
  width: 100%;
}
</style>