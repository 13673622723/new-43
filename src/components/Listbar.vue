
<template>
  <!-- 列表按钮 -->
  <div class="listbar">
    <router-link to="#">
      <div>{{label}}</div>
      <div class="tips">
        {{tips}}
        <span class="iconfont iconjiantou1"></span>
      </div>
    </router-link>
  </div>
</template>
<script>
export default {
  props: ["label", "tips"]
};
</script>

<style lang="less" scoped>
.listbar {
  padding-left: 25/360 * 100vw;
  height: 50/360 * 100vw;
  a {
    display: flex;
    justify-content: space-between;
    line-height: 50/360 * 100vw;
    border-bottom: 1px solid #eee;
    align-items: center;
    padding-right: 25/360 * 100vw;
  }
  .tips {
    color: #999;
    span {
      margin-left: 5px;
    }
  }
}
</style>