<template>
  <div>
    <hm-header>登录</hm-header>
    <hm-logo></hm-logo>
    <van-form @submit="login">
    <van-field
      v-model="username"
      name="用户名"
      label="账号"
      placeholder="请输入账号"
      :rules="rules.username"
    />
    <van-field
      v-model="password"
      type="password"
      name="密码"
      label="密码"
      placeholder="请输入密码"
      :rules="rules.password"
    />
    <div style="margin: 16px;">
      <van-button block type="info" native-type="submit">
        登录
      </van-button>
    </div>
    <p class="tips">已有账号？去<router-link to='/register'>注册</router-link></p>
  </van-form>
  </div>
</template>

<script>
// import axios from 'axios'
export default {
  created () {
    const { username, password } = this.$route.params
    this.username = username
    this.password = password
  },
  methods: {
    async login () {
      const res = await this.$axios.post('/login', {
        username: this.username,
        password: this.password
      })
      // console.log(res)
      const { statusCode, message, data } = res.data
      console.log(res.data)
      if (statusCode === 200) {
        this.$toast.success(message)
        localStorage.setItem('token', data.token)
        localStorage.setItem('userId', data.user.id)
        this.$router.push('./user')
      } else {
        this.$toast(message)
      }
    }
  },
  data () {
    return {
      username: '',
      password: '',
      rules: {
        username: [
          { required: true, message: '请填写用户名', trigger: 'onChange' },
          { pattern: /^\d{5,11}$/, message: '用户名长度是5-11位数字', trigger: 'onChange' }
        ],
        password: [
          { required: true, message: '密码不能为空', trigger: 'onChange' },
          { pattern: /^\d{3,9}$/, message: '密码长度是3-9位数字', trigger: 'onChange' }
        ]
      }
    }
  }
}
</script>

<style lang = 'less' scoped>
  .tips{
        text-align: right;
        font-size: 10px;
        padding: 10px;
        a{
          color: red;
        }
    }
</style>
