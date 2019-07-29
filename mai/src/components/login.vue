<template>
  <div class="login">
    <el-form
      :model="loginForm"
      status-icon
      :rules="rules"
      ref="loginForm"
      label-width="100px"
      class="demo-loginForm"
    >
      <h1 class="title">
        <i class="el-icon-user"></i>
        vuex登录
      </h1>

      <el-form-item label="用户" prop="username">
        <el-input type="text" v-model="loginForm.username" autocomplete="off"></el-input>
      </el-form-item>

      <el-form-item label="密码" prop="password">
        <el-input type="password" v-model="loginForm.password" autocomplete="off"></el-input>
      </el-form-item>

      <el-form-item>
        <el-button type="primary" @click="submitForm('loginForm')">登录</el-button>
        <el-button @click="resetForm('loginForm')">重置</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      loginForm: {
        // 登陆表单的数据
        username: "",
        password: ""
      },
      // 验证规则
      rules: {
        username: [
          { required: true, message: "请输入用户名", trigger: "blur" },
          { min: 3, max: 15, message: "长度在 3 到15 个字符", trigger: "blur" }
        ],
        password: [
          { required: true, message: "请输入密码", trigger: "blur" },
          { min: 3, max: 15, message: "长度在 3 到15 个字符", trigger: "blur" }
        ]
      }
    };
  },
  methods: {
    submitForm(formName) {
      // 取dom，前端所有验证通过了，才会传数据到后端
      this.$refs[formName].validate(valid => {
        if (valid) {
          //将用户名密码通过axios传到后端
          let that = this;
          //通过代理方式，将请求地址代理到888端口下，即解决跨域
          this.axios
            .post("/api/checklogin", {
              username: that.loginForm.username,
              password: that.loginForm.password
            })
            .then(response => {
              if (response.data.length) {
                console.log("接收后端响应请求的数据：", response.data);
                that.$message({
                  message: "恭喜你，登录成功",
                  type: "success"
                });
              } else {
                that.$message.error("登录失败");
              }
            });
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    },
    resetForm(formName) {
      this.$refs[formName].resetFields();
    }
  }
};
</script>
 
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style >
html,
body {
  margin: 0%;
  padding: 0%;
}
html,
body,
#app,
.login {
  height: 100%;
}
.login {
  display: flex;
  /* 水平居中，垂直居中 */
  justify-content: center;
  align-items: center;
}
.el-form {
  width: 400px;
  border: 1px solid #ccc;
  border-radius: 4px;
  padding: 0px 55px 10px 10px;
}
.el-form .title {
  font-size: 22px;
  color: #333;
  font-weight: bold;
  padding: 20px 0 20px 60px;
}
</style>
