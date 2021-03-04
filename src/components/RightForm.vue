<template>
  <main class="right-form-content">
    <header>{{ title || "设置" }}</header>
    <!-- 二级菜单未设置信息选择按钮组件 -->
    <SetOperation
      :info_data="info_data"
      v-if="info_data && info_data.menu_level === 2"
    />
    <!-- 一级菜单设置表单 -->
    <OneLevelMenuDetail
      :info_data="info_data"
      v-show="info_data && info_data.menu_level === 1"
    />
  </main>
</template>

<script>
import SetOperation from "@/components/children/SetOperation.vue";
import OneLevelMenuDetail from "@/components/children/OneLevelMenuDetail.vue";

export default {
  data() {
    return {
      info_data: undefined,
      title: "",
    };
  },
  props: {
    row_data: {
      type: Object,
      value: undefined,
    },
  },
  watch: {
    row_data(newValue, oldValue) {
      this.info_data = newValue;
      let title = [
        "设置",
        "设置动作（文本）",
        "设置动作（网页）",
        "设置动作（图文信息）",
        "设置动作（跳转小程序）",
      ];
      this.title = title[newValue.menu_type];
    },
  },
  components: {
    SetOperation,
    OneLevelMenuDetail,
  },
};
</script>

<style scoped>
.right-form-content {
  width: 100%;
}
.right-form-content header {
  width: 100%;
  height: 44px;
  display: block;
  text-align: left;
  border-bottom: 1px solid #eee;
  color: #e6a23c;
  padding-left: 30px;
  /* padding: 11px 3px; */
}
</style>