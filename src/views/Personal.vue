<template>
  <div class="geren">
    <!-- 头部 -->
    <NavigateBar title="个人中心" :showHome="true" />
    <!-- 给头部加router-link跳转到编辑页 -->
    <router-link to="/edit-profile">
      <div class="header">
        <div class="avatar">
          <img :src="$axios.defaults.baseURL + userInfo.head_img" />
        </div>
        <div class="profile">
          <div>
            <span class="iconfont iconxingbienan" v-if="userInfo.gender === 1"></span>
            <span class="iconfont iconxingbienv" v-if="userInfo.gender === 0"></span>
            {{userInfo.nickname}}
          </div>
          <!-- 日期渲染 -->
          <p>{{moment(userInfo.create_data).format('YYYY-MM-DD')}}</p>
        </div>
        <span class="arrow iconfont iconjiantou1"></span>
      </div>
    </router-link>
    <Listbar v-for="(item,index) in rows" :key="index" :label="item.label" :tips="item.tips" />
    <!-- click.native 这个事件会给Listbar组件 最外层的div强制注册点击事件 -->
    <Listbar @click.native="handleClick" label="退出" />
  </div>
</template>

<script>
// 封装个人中心列表组件
// 导入列表按钮栏的组件，import后面接上组件变量名
// 再去个人中心渲染
import Listbar from "@/components/Listbar";
// 引入第三方的日期处理的工具库
import moment from "moment";
import NavigateBar from "@/components/NavigateBar";
export default {
  data() {
    return {
      rows: [
        { label: "我的关注", tips: "关注的用户" },
        { label: "我的跟帖", tips: "跟帖/回复" },
        { label: "我的收藏", tips: "文章/视频" }
      ],
      userInfo: {},
      //   日期处理的工具库，需要绑定在data种
      moment: moment
    };
  },
  // 注册组件
  components: {
    Listbar,
    NavigateBar
  },
  //   组件加载完成触发
  mounted() {
    // 从本地获取token和id
    const jsonStr = localStorage.getItem("userInfo");
    const userJson = JSON.parse(jsonStr);
    // 发起异步的请求
    this.$axios({
      url: "/user/" + userJson.user.id,
      headers: {
        Authorization: userJson.token
      }
    }).then(res => {
      //   console.log(res);
      // 结构出用户的数据
      const { data } = res.data;
      //   赋值给userInfo
      this.userInfo = data;
    });
  },
  methods: {
    handleClick() {
      //   数据  询问用户是否退出
      this.$dialog
        .confirm({
          title: "提示",
          message: "确定退出吗"
        })
        .then(() => {
          localStorage.removeItem("userInfo");
          this.$router.replace("/login");
        })
        .catch(() => {});
    }
  }
};
</script>

<style lang="less" scoped>
.header {
  padding: 25 / 360 * 100vw;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-bottom: 5px solid #eee;
  .avatar {
    img {
      display: block;
      width: 70/360 * 100vw;
      height: 70/360 * 100vw;
      border-radius: 50%;
    }
  }
  .profile {
    flex: 1;
    padding-left: 20/360 * 100vw;
    line-height: 1.5;
    p {
      color: #999;
    }
    .iconxingbienan {
      color: blue;
    }
    .iconxingbienv {
      color: red;
    }
  }
}
</style>