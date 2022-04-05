<template>
  <div>
    <el-form
      v-loading="loading"
      element-loading-text="正在登录..."
      element-loading-spinner="el-icon-loading"
      element-loading-background="rgba(0, 0, 0, 0.8)"
      :rules="rules"
      ref="loginForm"
      :model="loginForm"
      class="loginContainer"
    >
      <h3 class="loginTitle">真寻的后台</h3>
      <el-form-item prop="username">
        <el-input
          type="text"
          v-model="loginForm.username"
          placeholder="请输入用户名"
        ></el-input>
      </el-form-item>
      <el-form-item prop="password">
        <el-input
          type="password"
          @keyup.enter.native="submitLogin"
          v-model="loginForm.password"
          placeholder="请输入密码"
          autofocus="false"
        ></el-input>
      </el-form-item>
      <div class="tip">
        <!-- 记住我 -->
        <el-checkbox v-model="checked" class="rememberMe">记住我</el-checkbox>
      </div>
      <el-button type="primary" style="width: 100%" @click="submitLogin"
        >登录</el-button
      >
    </el-form>
  </div>
</template>

<script>
import qs from "qs";
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "Login",
  data() {
    return {
      loginForm: {
        username: "",
        password: "",
      },
      rules: {
        username: [
          { required: true, message: "请输入用户名", trigger: "blur" },
        ],
        password: [{ required: true, message: "请输入密码", trigger: "blur" }],
      },
      loading: false,
      checked: false,
    };
  },
  mounted() {
    this.getCookie();
  },
  methods: {
    submitLogin() {
      this.$refs.loginForm.validate((valid) => {
        if (valid) {
          this.loading = true;
          if (this.checked) {
            this.setCookie(this.loginForm.username, this.loginForm.password, 7);
          }
          else {
            this.setCookie("", "", -1);
          }
          this.postRequest(
            "/webui/login",
            qs.stringify({
              username: this.loginForm.username,
              password: this.loginForm.password,
            }),
            { headers: { "Content-Type": "application/x-www-form-urlencoded" } }
          ).then((resp) => {
            if (resp) {
              // this.loading = false;
              const tokenStr = resp.token_type + " " + resp.access_token;
              window.sessionStorage.setItem("tokenStr", tokenStr);
              // // 页面跳转
              let path = this.$route.query.redirect;
              this.$router.replace(
                path == "/" || path == undefined ? "/home" : path
              );
            }
          });
        } else {
          this.$message.error("请输入所有字段！");
          return false;
        }
        this.loading = false;
      });
    },
    // 设置cookie
    setCookie(username, password, days) {
      let date = new Date(); // 获取时间
      date.setTime(date.getTime() + 24 * 60 * 60 * 1000 * days); // 保存的天数
      // 字符串拼接cookie
      window.document.cookie =
        "username" + "=" + username + ";path=/;expires=" + date.toGMTString();
      window.document.cookie =
        "password" + "=" + password + ";path=/;expires=" + date.toGMTString();
    },
    // 读取cookie 将用户名和密码回显到input框中
    getCookie() {
      if (document.cookie.length > 0) {
        let arr = document.cookie.split("; "); //分割成一个个独立的“key=value”的形式
        for (let i = 0; i < arr.length; i++) {
          let arr2 = arr[i].split("="); // 再次切割，arr2[0]为key值，arr2[1]为对应的value
          if (arr2[0] === "username") {
            this.loginForm.username = arr2[1];
          } else if (arr2[0] === "password") {
            // this.loginForm.password = Base64.decode(arr2[1]);// base64解密
            this.loginForm.password=arr2[1];
            this.checked = true;
          }
        }
      }
    }
  },
};
</script>

<style>
.loginContainer {
  border-radius: 15px;
  background-clip: padding-box;
  margin: 12% auto;
  width: 350px;
  padding: 15px 35px 15px 35px;
  background: #fff;
  border: 1px solid #eaeaea;
  box-shadow: 0 0 25px #caca6c;
}

.loginTitle {
  margin: 10px auto 40px auto;
  text-align: center;
}

.loginRemember {
  text-align: left;
  margin: 0px 0px 15px 0px;
}

.el-form-item__content {
  display: flex;
  align-items: center;
}
</style>