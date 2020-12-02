<template>
  <div class="login-container">
    <div class="login-box">
      <!-- 头像区域 -->
      <div class="avatar-box">
        <img src="../assets/logo.png" />
      </div>
      <!-- 登陆表单区域 -->
      <el-form
        ref="loginFormRef"
        :model="loginForm"
        :rules="loginFormRules"
        label-width="0px"
        class="login-form"
      >
        <!-- 用户名 -->
        <el-form-item prop="username">
          <el-input
            v-model="loginForm.username"
            prefix-icon="iconfont icon-user"
            placeholder="用户名"
          ></el-input>
        </el-form-item>
        <!-- 密码 -->
        <el-form-item prop="password">
          <el-input
            v-model="loginForm.password"
            prefix-icon="iconfont icon-3702mima"
            placeholder="密码"
            type="password"
          ></el-input>
        </el-form-item>
        <!-- 按钮 -->
        <el-form-item class="btns">
          <el-button type="primary" @click="onSumbitLoginForm">登陆</el-button>
          <el-button type="info" @click="onResetLoginForm">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  name: 'login',
  data: () => ({
    // 这是登录表单的数据绑定对象
    loginForm: {
      username: 'admin',
      password: '123456'
    },
    // 登录表单验证的规则
    loginFormRules: {
      username: [
        {
          required: true,
          message: '请输入用户名！',
          trigger: 'blur'
        },
        {
          min: 3,
          max: 10,
          message: '长度在3到10个字符',
          trigger: 'blur'
        }
      ],
      password: [
        {
          required: true,
          message: '请输入密码！',
          trigger: 'blur'
        },
        {
          min: 6,
          max: 15,
          message: '长度在6到15个字符',
          trigger: 'blur'
        }
      ]
    }
  }),
  methods: {
    onResetLoginForm() {
      // 表单重置
      this.$refs.loginFormRef.resetFields()
    },
    onSumbitLoginForm() {
      // 表单验证
      this.$refs.loginFormRef.validate(async (validate) => {
        // 校验表单是否成功
        if (!validate) return null
        // 解构获取的数据
        const { data: res } = await this.$http.post('login', this.loginForm)
        // 校验是否发生错误
        if (res.meta.status !== 200) return this.$message.error('登陆失败')
        // 提示文本
        this.$message.success('登陆成功')
        // 设置token
        window.sessionStorage.setItem('token', res.data.token)
        // 跳转路由
        this.$router.push('/home')
      })
    }
  }
}
</script>

<style lang="less" scoped>
.login-container {
  background-color: #2b4b6b;
  height: 100%;
}

.login-box {
  width: 450px;
  height: 300px;
  background-color: #fff;
  border-radius: 3px;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);

  .avatar-box {
    height: 130px;
    width: 130px;
    border: 1px solid #eee;
    border-radius: 50%;
    padding: 10px;
    box-shadow: 0 0 10px #ddd;
    position: absolute;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: #fff;

    img {
      width: 100%;
      height: 100%;
      border-radius: 50%;
      background-color: #eee;
    }
  }
}

.login-form {
  position: absolute;
  bottom: 0;
  width: 100%;
  padding: 0 20px;
  box-sizing: border-box;
}

.btns {
  display: flex;
  justify-content: flex-end;
}
</style>
