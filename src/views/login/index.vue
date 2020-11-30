<template>
  <div class="login-container">
    <van-nav-bar
      title="注册 / 登录"
      left-arrow
      class="app-nav-bar"
      @click-left="$router.back()"
    />

    <!-- 登录表单 -->
    <van-form
      :show-error="false"
      :show-error-message="false"
      validate-first
      @submit="onLogin"
      @failed="onFailed"
    >
      <van-field
        v-model="user.mobile"
        icon-prefix="toutiao"
        left-icon="shouji"
        name="手机号"
        placeholder="请输入手机号"
        :rules="formRules.mobile"
      />
      <van-field
        v-model="user.code"
        clearable
        icon-prefix="toutiao"
        name="验证码"
        left-icon="yanzhengma"
        placeholder="请输入验证码"
        :rules="formRules.code"
      >
        <template #button>
          <van-button
            class="send-btn"
            size="mini"
            round
          >发送验证码</van-button>
        </template>
      </van-field>
      <div class="login-btn-wrap">
      <van-button
        class="login-btn"
        type="info"
        block
      >登录</van-button>
    </div>
    </van-form>
    <!-- /登录表单 -->
  </div>
</template>

<script>
import { login } from '@/api/user'
export default {
  name: 'HomeIndex',
  components: {},
  props: {},
  data () {
    return {
      user: {
        mobile: '13911111111', // 手机号
        code: '246810' // 验证码
      },
      formRules: {
        mobile: [
          { required: true, message: '请输入手机号' },
          { pattern: /^1[3|5|7|8|9]\d{9}$/, message: '手机号格式错误' }
        ],
        code: [
          { required: true, message: '请输入验证码' },
          { pattern: /^\d{6}$/, message: '验证码格式错误' }
        ]
      }
    }
  },
  computed: {},
  watch: {},
  created () {},
  mounted () {},
  methods: {
    onFailed (error) {
      alert('ok')
      if (error.errors[0]) {
        this.$toast({
          message: error.errors[0].message, // 提示消息
          position: 'top' // 防止手机键盘太高看不见提示消息
        })
      }
    },
    async onLogin () {
      // 1. 找到数据接口
      // 2. 封装请求方法
      // 3. 请求调用登录
      this.$toast.loading({
        message: '登录中...', // 提示文本
        forbidClick: true, // 禁止背景点击
        duration: 0 // 展示时长(ms)，值为 0 时，toast 不会消失
      })
      try {
        const { data } = await login(this.user)
        // 4. 处理响应结果
        this.$toast.success('登录成功')
        this.$store.commit('setUser', data.data)
        this.$router.back()
      } catch (err) {
        console.log(err)
        this.$toast.fail('登录失败，手机号或验证码错误')
      }
    }
  }
}
</script>

<style scoped lang="less">
.login-container {
  .send-btn {
    width: 76px;
    height: 23px;
    background-color: #ededed;
    .van-button__text {
      font-size: 11px;
      color: #666666;
    }
  }
  .login-btn-wrap {
    padding: 26px 16px;
    .login-btn {
      background-color: #6db4fb;
      border: none;
      .van-button__text {
        font-size: 15px;
      }
    }
  }
}
</style>
