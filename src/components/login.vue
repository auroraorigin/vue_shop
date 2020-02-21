<template>
  <div class="login_container">
    <div class="login_box">
      <div class="avatar_box">管理员登录</div>
      <el-form
        ref="loginFormRef"
        :model="loginForm"
        :rules="loginFormRules"
        label-width="0px"
        class="form"
      >
        <el-form-item prop="username">
          <el-input
            v-model="loginForm.username"
            prefix-icon="iconfont icon-user"
            clearable
            maxlength="10"
            placeholder="用户名"
          ></el-input>
        </el-form-item>
        <el-form-item prop="password">
          <el-input
            v-model="loginForm.password"
            prefix-icon="iconfont icon-3702mima"
            type="password"
            maxlength="15"
            placeholder="密码"
            show-password
          ></el-input>
        </el-form-item>
        <el-form-item>
          <el-button class="but" type="primary" @click="login">登录</el-button>
          <!-- :loading="true" -->
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      loginForm: {
        username: 'admin',
        password: '123456'
      },
      loginFormRules: {
        username: [
          { required: true, message: '请输入用户名称', trigger: 'blur' },
          {
            min: 3,
            max: 10,
            message: '用户名长度在 3 到 10 个字符',
            trigger: 'blur'
          }
        ],
        password: [
          { required: true, message: '请输入登录密码', trigger: 'blur' },
          {
            min: 6,
            max: 15,
            message: '密码长度在 6 到 15 个字符',
            trigger: 'blur'
          }
        ]
      }
    }
  },
  methods: {
    login() {
      this.$refs.loginFormRef.validate(async valid => {
        if (!valid) {
          return
        }
        const { data: res } = await this.$http.post('login', this.loginForm)
        if (res.meta.status !== 200) {
          return this.$message.error('登录失败')
        }
        this.$message.success('登陆成功')
        console.log(res)
        window.sessionStorage.setItem('token', res.data.token)
        this.$router.push('/home')
      })
    }
  }
}
</script>

<style lang="less" scoped>
.login_container {
  background-color: #293a4a;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.login_box {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 380px;
  height: 400px;
  background-color: #293a4a;
  border-radius: 6px;

  .avatar_box {
    font-size: 23px;
    color: #e9f3f4;
    margin-bottom: 30px;
  }
  .el-input /deep/ .el-input__inner{
      background-color: #243443;
      color: #e9f3f4;
      border-color: #384959;
    }

  .form {
    width: 100%;
    .but {
      width: 100%;
      background-color: #79c0fe;
      color: #e9f3f4;
    }
  }
}

</style>
