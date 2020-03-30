<template>
  <!-- 顶部返回的导航条 -->
  <div class="navigate-bar">
    <span class="iconfont iconjiantou2" @click="$router.back()"></span>
    <strong>{{title}}</strong>
    <span class="iconfont iconshouye" @click="$router.push('/')" v-if="showHome"></span>
    <!-- 接着v-if后面使用 -->
    <span v-else class="iconfont iconshouye" style="opacity: 0"></span>
  </div>
</template>

<script>
export default {
  // 把个人中心动态修改，是否是文字和首页的图标
  //   showHome如果是true就显示，如果falss就隐藏
  props: ["title", "showHome"]
};
</script>

<style lang="less" scoped>
.navigate-bar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  line-height: 48 /360 * 100vw;
  padding: 20 /360 * 100vw;
  .iconshouye {
    font-size: 20px;
  }
}
</style>