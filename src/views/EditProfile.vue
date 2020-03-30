<template>
  <div>
    <NavigateBar title="编辑资料" />
    <!-- 头像 -->
    <div class="acatat">
      <img :src="$axios.defaults.baseURL + userInfo.head_img" />
      <!-- 复制的 -->
      <van-uploader :after-read="afterRead" class="uploader" />
    </div>
    <Listbar label="昵称" :tips="userInfo.nickname" @click.native="show = true" />
    <!-- 编辑昵称的弹窗 vant第三方复制过来的-->
    <van-dialog v-model="show" title="修改昵称" show-cancel-button @confirm="handleChangeNickname">
      <!-- vant第三方复制过来的--输入框的代码 -->
      <van-field v-model="nickname" placeholder="请输入用户名" />
    </van-dialog>

    <Listbar label="密码" tips="******" @click.native="showPassword = true" />

    <van-dialog
      v-model="showPassword"
      title="修改密码"
      show-cancel-button
      @confirm="handleChangePassword"
    >
      <van-field v-model="password" placeholder="请输入新的密码" type="password" />
    </van-dialog>

    <Listbar label="性别" :tips="['女','男'][userInfo.gender]" @click.native="showGender=true" />

    <van-action-sheet
      v-model="showGender"
      close-on-click-action
      :actions="actions"
      @select="onSelect"
    />
  </div>
</template>

<script>
import NavigateBar from "@/components/NavigateBar";
import Listbar from "@/components/Listbar";
export default {
  data() {
    return {
      userInfo: {},
      userJson: {},
      //   vant第三方复制过来调用的
      show: false,
      showPassword: false,
      showGender: false,
      actions: [
        { name: "男", value: 1 },
        { name: "女", value: 0 }
      ],
      nickname: "",
      password: ""
    };
  },
  components: {
    NavigateBar,
    Listbar
  },
  mounted() {
    const userJson = JSON.parse(localStorage.getItem("userInfo"));
    this.userJson = userJson;
    this.$axios({
      url: "/user/" + userJson.user.id,
      headers: {
        Authorization: userJson.token
      }
    }).then(res => {
      console.log(res);
      const { data } = res.data;
      this.userInfo = data;
      this.nickname = data.nickname;
      this.password = data.password;
    });
  },
  methods: {
    afterRead(file) {
      // 此时可以自行将文件上传至服务器
      console.log(file);
      const fd = new FormData();
      fd.append("file", file.file);
      // 开始上传
      this.$axios({
        url: "/upload",
        // post请求
        method: "POST",
        // 添加头信息
        headers: {
          Authorization: this.userJson.token
        },
        data: fd
      }).then(res => {
        console.log(res);

        // url就是图片的路径
        const { url } = res.data.data;
        // 替换掉当前的头像路径
        this.userInfo.head_img = url;
        // 图片上传成功之后调用编辑用户信息的方法
        this.handleEdit({
          head_img: url
        });
      });
    },
    handleEdit(data) {
      this.$axios({
        url: "/user_update/" + this.userInfo.id,
        method: "POST",
        // 添加头信息
        headers: {
          Authorization: this.userJson.token
        },
        data
      }).then(res => {
        console.log(res);

        // console.log(res)
        this.$toast.success("修改成功");
      });
    },
    // 点击确定，修改昵称
    handleChangeNickname() {
      this.handleEdit({ nickname: this.nickname });
      this.userInfo.nickname = this.nickname;
    },
    handleChangePassword() {
      this.handleEdit({ password: this.password });
    },
    onSelect(item) {
      this.handleEdit({ gender: item.value });
      this.userInfo.gender = item.value;
    }
  }
};
</script>

<style lang="less" scopde>
.acatat {
  display: flex;
  padding: 20 / 360 * 100vw;
  justify-content: center;
  align-items: center;
  position: relative;
  img {
    width: 100 / 360 * 100vw;
    height: 100 / 360 * 100vw;
    border-radius: 50%;
  }
  .uploader {
    position: absolute;
    width: 100 / 360 * 100vw;
    height: 100 / 360 * 100vw;
    left: 50%;
    top: 50%;
    transform: translateX(-50 / 360 * 100vw) translateY(-50 /3 60 * 100vw);
    opacity: 0;
  }
}
</style>