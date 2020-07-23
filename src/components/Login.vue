<template>
    <div class="login_box">
        <div class="login_form">
            <div class="logo">
                <img src="../assets/logo.png" alt="">
            </div>
            <!-- 登录表单 ref唯一标识 -->
            <el-form ref="loginFormRef" label-width="0px" :model="loginForm" :rules="loginRules">
                <!-- 输入框 -->
                <el-form-item prop="username">
                    <el-input v-model="loginForm.username"></el-input>
                </el-form-item>
                <el-form-item prop="password">
                    <el-input type="password" v-model="loginForm.password"></el-input>
                </el-form-item>
                <!-- 两个按钮 -->
                <el-form-item class="btns">
                    <el-button type="primary" @click="login">登录</el-button>
                    <el-button type="info" @click="resetLoginFrom">重置</el-button>
                </el-form-item>
            </el-form>
        </div>
    </div>
</template>

<script>
export default {
  data () {
    return {
      // 登录表单的数据绑定对象
      loginForm: {
        username: '',
        password: ''
      },
      // 表单验证规则对象
      loginRules: {
        username: [
          { required: true, message: '请输入用户名', trigger: 'blur' },
          { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' },
          { min: 6, max: 18, message: '长度在 6到18 个字符', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    // 重置表单
    resetLoginFrom () {
      this.$refs.loginFormRef.resetFields()
    },
    // 登录处理
    login () {
      // 进行预校验
      this.$refs.loginFormRef.validate(async (valid) => {
        if (!valid) return
        // 发请求调接口
        const { data: res } = await this.$http.post('login', this.loginForm)
        if (res.meta.status !== 200) {
          return this.$message({
            message: '登录失败',
            type: 'error'
          })
        }
        this.$message.success('登录成功')
        // 1 把token保存起来 setItem设置某一项 最好为token
        window.sessionStorage.setItem('token', res.data.token)
        console.log(window.sessionStorage.getItem('token'))
        // 2 跳转主页
        this.$router.push('/home')
      })
    }
  }
}
</script>

<!--scoped 表示当前组件下有效-->
<style lang="less" scoped>
    .login_box{
        height: 100%;
        background-color: #2b4b6b;
    }
    .login_form{
        width: 450px;
        height: 300px;
        background-color: #fff;
        border-radius: 4px;
        position: absolute;
        left: 50%;
        top: 50%;
        transform: translate(-50%,-50%);
    }
    .login_form .logo{
        position: absolute;
        left: 50%;
        transform: translate(-50%,-50%);
        width: 140px;
        height: 140px;
        border-radius:50% ;
        padding: 10px;
        box-shadow: 0 0 10px #ddd;
        background-color:#fff ;
        img{
            width: 100%;
            height: 100%;
            border-radius: 50%;
        }
    }
    .login_form .el-form{
        position: absolute;
        bottom: 0;
        padding: 0 20px;
        width: 100%;
        box-sizing: border-box;
    }
    .btns{
        display: flex;
        justify-content: flex-end;
    }
</style>
