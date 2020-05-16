<template>
  <div class="hello">
    <el-form>
      <el-row>
        <el-col :span="6">
          <div>姓名：
                    <el-tag>{{memberInfo.name}}</el-tag>
          </div>
        </el-col>
        <el-col :span="6">
          <div>年龄：
                    <el-tag>{{memberInfo.age}}</el-tag>
          </div>
        </el-col>
      </el-row>
    </el-form>
  </div>
</template>

<script>
import {Message} from 'element-ui'
export default {
  name: 'Home',
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
      },
      memberInfo: null
    }
  },
  mounted () {
    this.initMemberInfo()
  },
  methods: {
    // updateVerifyCode () {
    //   this.vcUrl = '/verifyCode?time=' + new Date()
    // },
    initMemberInfo () {
      // debugger
      this.loading = true
      this.$axios.post('/member').then((resp) => {
        this.loading = false
        if (resp) {
          debugger
          // this.$store.commit('INIT_CURRENTHR', resp.obj)
          this.memberInfo = resp.data
        }
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
    }
  }
}
</script>
