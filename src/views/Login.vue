<template>
  <div>
    <el-form
      :rules="rules"
      ref="loginForm"
      v-loading="loading"
      element-loading-text="正在登录..."
      element-loading-spinner="el-icon-loading"
      element-loading-background="rgba(0, 0, 0, 0.8)"
      :model="loginForm"
      class="loginContainer"
    >
      <h3 class="loginTitle">系统登录</h3>
      <el-form-item prop="username">
        <el-input
          size="normal"
          type="text"
          v-model="loginForm.username"
          auto-complete="off"
          placeholder="请输入用户名"
        ></el-input>
      </el-form-item>
      <el-form-item prop="password">
        <el-input
          size="normal"
          type="password"
          v-model="loginForm.password"
          auto-complete="off"
          placeholder="请输入密码"
        ></el-input>
      </el-form-item>
      <!-- <el-form-item prop="code">
        <el-input
          size="normal"
          type="text"
          v-model="loginForm.code"
          auto-complete="off"
          placeholder="点击图片更换验证码"
          @keydown.enter.native="submitLogin"
          style="width: 250px;"
        ></el-input>
        <img
          :src="vcUrl"
          @click="updateVerifyCode"
          alt=""
          style="cursor: pointer;"
        />
      </el-form-item> -->
      <!-- <el-checkbox
        size="normal"
        class="loginRemember"
        v-model="checked"
      ></el-checkbox> -->
      <el-button
        size="normal"
        type="primary"
        style="width: 100%;"
        @click="submitLogin"
        >登录</el-button
      >
    </el-form>
  </div>
</template>

<script>
import {Message} from 'element-ui'
export default {
  name: 'Login',
  data () {
    return {
      loading: false,
      // vcUrl: '/verifyCode?time=' + new Date(),
      loginForm: {
        username: 'admin',
        password: '123'
      },
      checked: true,
      rules: {
        username: [
          { required: true, message: '请输入用户名', trigger: 'blur' }
        ],
        password: [{ required: true, message: '请输入密码', trigger: 'blur' }],
        code: [{ required: true, message: '请输入验证码', trigger: 'blur' }]
      }
    }
  },
  methods: {
    // updateVerifyCode () {
    //   this.vcUrl = '/verifyCode?time=' + new Date()
    // },
    submitLogin () {
      // debugger
      this.$refs.loginForm.validate((valid) => {
        if (valid) {
          this.loading = true
          this.$axios.post('/login', {username: 'admin', password: 'admin'}).then((resp) => {
            this.loading = false
            if (resp) {
              debugger
              // this.$store.commit('INIT_CURRENTHR', resp.obj)
              window.sessionStorage.setItem('user', JSON.stringify(resp.obj))
              let path = this.$route.query.redirect
              this.$router.replace(
                path === '/' || path === undefined ? '/home' : path
              )
            }
            // else {
            //   this.vcUrl = '/verifyCode?time=' + new Date()
            // }
          }).catch((error) => {
            this.loading = false
            switch (error.response.status) {
              case 400: Message.error({message: '请求错误(400)'}); break
              case 401: Message.error({message: '未授权，请重新登录(401)'}); break
              case 403: Message.error({message: '拒绝访问(403)'}); break
              case 404: Message.error({message: '请求出错(404)'}); break
              case 408: Message.error({message: '请求超时(408)'}); break
              case 500: Message.error({message: '服务器错误(500)'}); break
              case 501: Message.error({message: '服务未实现(501)'}); break
              case 502: Message.error({message: '网络错误(502)'}); break
              case 503: Message.error({message: '服务不可用(503)'}); break
              case 504: Message.error({message: '网络超时(504)'}); break
              case 505: Message.error({message: 'HTTP版本不受支持(505)'}); break
              default: Message.error({message: 'system error!'})
            }
          })
          // axios.post('/api/member', this.loginForm).then((resp) => {
          //   debugger
          // })
        } else {
          return false
        }
      })
    }
  }
}
</script>
