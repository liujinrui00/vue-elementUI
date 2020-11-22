<template>
  <div class="login_container">
    <div class="login_box">
      <div class="loginLeft"></div>
      <div class="loginRight">
        <div class="rightContent">
          <div class="header">
            <h2>登录</h2>
            <span style="position:relative;top:30px">没有账号？点此注册</span>
          </div>
          <div class="loginForm">
            <el-form ref="form" :model="loginForm">
              <el-form-item>
                <el-input v-model="loginForm.username"></el-input>
              </el-form-item>
              <el-form-item>
                <el-input v-model="loginForm.password"></el-input>
              </el-form-item>
            </el-form>
          </div>
          <div class="button">
            <div class="buttonTop">
              <el-checkbox v-model="checked">记住我</el-checkbox>
              <span>短信验证登录</span>
            </div>
            <div class="loginBtn">
              <el-button
                style="background-color:#fe7300;width:100%;height:100%;font-size:18px;color:#ffffff"
              >登录</el-button>
            </div>
            <div class="forgetPw">
              <span>已有账号，忘记密码？</span>
            </div>
          </div>
          <div class="loginMode">
            <img src="../../public/img/icons/loginMode.png" />
          </div>
          <div class="footer">
            <img src="../../public/img/icons/QQ.png" />
            <img src="../../public/img/icons/WX.png" />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // 这是登录表单的数据绑定对象
      loginForm: {
        username: "admin",
        password: "123456"
      },
      // 这是表单的验证规则对象
      loginFormRules: {
        // 验证用户名是否合法
        username: [
          { required: true, message: "请输入登录名称", trigger: "blur" },
          { min: 3, max: 10, message: "长度在 3 到 10 个字符", trigger: "blur" }
        ],
        // 验证密码是否合法
        password: [
          { required: true, message: "请输入登录密码", trigger: "blur" },
          { min: 6, max: 15, message: "长度在 6 到 15 个字符", trigger: "blur" }
        ]
      }
    };
  },
  methods: {
    // 点击重置按钮，重置登录表单
    resetLoginForm() {
      // console.log(this);
      this.$refs.loginFormRef.resetFields();
    },
    login() {
      this.$refs.loginFormRef.validate(async valid => {
        if (!valid) return;
        const { data: res } = await this.$http.post("login", this.loginForm);
        if (res.meta.status !== 200) return this.$message.error("登录失败！");
        this.$message.success("登录成功");
        // 1. 将登录成功之后的 token，保存到客户端的 sessionStorage 中
        //   1.1 项目中出了登录之外的其他API接口，必须在登录之后才能访问
        //   1.2 token 只应在当前网站打开期间生效，所以将 token 保存在 sessionStorage 中
        console.log(res);

        window.sessionStorage.setItem("token", res.data.token);
        // 2. 通过编程式导航跳转到后台主页，路由地址是 /home
        this.$router.push("/home");
      });
    }
  }
};
</script>

<style lang="less" scoped>
.login_container {
  background-color: #ffffff;
  height: 100%;
}

.login_box {
  width: 1000px;
  height: 575px;
  background-color: #fff;
  border-radius: 3px;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  display: flex;
  .loginLeft {
    width: 50%;
    height: 100%;
    background-color: #5c6c84;
  }
  .loginRight {
    width: 50%;
    height: 100%;
    background-color: blue;
    .rightContent {
      width: 100%;
      height: 100%;
      margin: auto;
      background-color: #ffffff;
      box-shadow: 1px 1px 6px #333333;
      .header {
        display: flex;
        justify-content: space-between;
        position: relative;
        top: 40px;
        width: 80%;
        margin: 0 auto;
      }
      .loginForm {
        width: 80%;
        margin: 40px auto;
      }
      .button {
        width: 80%;
        margin: -10px auto;
        .buttonTop {
          display: flex;
          justify-content: space-between;
          margin: 0 auto;
        }
        .loginBtn {
          width: 100%;
          height: 40px;
          margin-top: 15px;
        }
        .forgetPw {
          width: 100%;
          display: flex;
          justify-content: center;
          align-items: center;
          margin-top: 20px;
        }
      }
      .loginMode {
        margin-top: 60px;
        display: flex;
        justify-content: center;
      }
      .footer {
        width: 50%;
        display: flex;
        justify-content: center;
        justify-content: space-between;
        /* align-items: center; */
        margin: 20px auto;
      }
    }
  }
}
</style>
