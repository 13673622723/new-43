<template>
  <div class="dly">
    <!-- 叉叉  返回上一页 -->
    <div class="back-btn">
      <span class="iconfont iconicon-test"></span>
    </div>
    <!-- logo -->
    <div class="log">
      <span class="iconfont iconnew"></span>
    </div>
    <!-- 表单 -->
    <!-- <div class="form">
      <div class="form-item">
        <input placeholder="手机号码" v-model="form.username" />
      </div>
      <div class="form-item">
        <input type="password" placeholder="请输入密码" v-model="form.password" />
      </div>
      <div class="form-item">
        <button @click="handleClick">登录</button>
      </div>
    </div>-->
    <!-- van-form表单的组件   @submit 点击按钮的事件 -->
    <van-form @submit="onSubmit" class="form">
      <van-field
        v-model="form.username"
        name="用户名或手机号码"
        placeholder="用户名或手机号码"
        :rules="[{required:true,message:'请填写用户名或手机号码'}]"
      />
      <van-field
        v-model="form.nickname"
        name="昵称"
        placeholder="请输入昵称"
        :rules="[{required:true,message:'请填写昵称'}]"
      />
      <van-field
        v-model="form.password"
        type="password"
        name="密码"
        placeholder="密码"
        :rules="[{required:true,message:'请填写密码'}]"
      />
      <div>
        <van-button round block type="info" native-type="submit">注册</van-button>
      </div>
    </van-form>
    <router-link to="/login">
      <van-button round block class="zhece">登录</van-button>
    </router-link>
  </div>
</template>

<script>
export default {
  data() {
    return {
      form: {
        username: "",
        password: "",
        nickname: ""
      }
    };
  },
  methods: {
    // 点击请求数据，
    // handleClick() {
    //   // this.$toast("登录成功");
    //   // 相当于ajax请求
    //   this.$axios({
    //     // 接口地址
    //     url: "http://hmtoutiao-api.atlansic.com/login",
    //     // 这里的method没有s，注意！！！
    //     // 请求方式
    //     method: "POST",
    //     // 请求参数
    //     data: this.form
    //     // 下面的是接收 相当于ajax的success
    //     // 成功的回调函数
    //   }).then(res => {
    //     // 获取返回的提示信息
    //     console.log(res);
    //     const { message } = res.data;
    //     // 插件 请提示
    //     this.$toast.success(message);
    //   });
    // },
    // 提交表单的时候触发的事件
    onSubmit(value) {
      console.log("submit", this.form);
      // this.$toast("登录成功");
      // 相当于ajax请求
      this.$axios({
        // 接口地址
        url: "/register",
        // 这里的method没有s，注意！！！
        // 请求方式
        method: "POST",
        // 请求参数
        data: this.form
        // 下面的是接收 相当于ajax的success
        // 成功的回调函数
      }).then(res => {
        // 获取返回的提示信息
        console.log(res);
        const { message } = res.data;
        // 插件 请提示
        this.$toast.success(message);
      });
      // 不要这个value
      // console.log("submit", value);
    }
  }
};
</script>

<style lang="less" scoped>
.dly {
  padding: 25/360 * 100vw;
}
.back-btn {
  font-size: 27/360 * 100vw;
  line-height: 0;
}
.log {
  text-align: center;
  margin-top: 20/360 * 100vw;
}
.log span {
  font-size: 126/360 * 100vw;
  color: #cc3300;
}
// .form-item {
//   margin-top: 25/360 * 100vw;
// }
// .form-item input {
//   width: 100%;
//   height: 44 / 360 * 100vw;
//   box-sizing: border-box;
//   line-height: 44 / 360 * 100vw;
//   background: none;
//   border: none;
//   border-bottom: 1px solid #999;
//   outline: none;
//   font-size: 16px;
// }
.form-item button {
  width: 100%;
  height: 48/360 * 100vw;
  border-radius: 48px;
  background-color: #cc3300;
  color: #fff;
  border: none;
  outline: none;
  font-size: 16px;
}
.form .van-cell {
  padding: 10px 0;
  font-size: 20/360 * 100vw;
  margin-bottom: 20/360 * 100vw;
}
.form .van-cell:not(:last-child)::after {
  border-bottom: 1px solid #333;
  left: 0;
}
.form .van-button--info {
  background-color: #cc3300;
  border: 1px solid #cc3300;
  margin-top: 50/360 * 100vw;
}
.zhece {
  margin-top: 20/360 * 100vw;
}
</style>