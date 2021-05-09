<template>
  <!-- 公共头 -->
  <header id="header">
    <section class="container">
      <!-- <h1 id="logo">
        <a href="#" title="谷粒学院">
          <img src="~/assets/img/logo.png" width="100%" alt="谷粒学院">
        </a>
      </h1> -->
      <div class="h-r-nsl">
        <ul class="nav">
          <router-link to="/" tag="li" active-class="current" exact>
            <a>首页</a>
          </router-link>
          <router-link to="/course" tag="li" active-class="current">
            <a>课程</a>
          </router-link>
          <router-link to="/teacher" tag="li" active-class="current">
            <a>名师</a>
          </router-link>
          <!-- <router-link to="/article" tag="li" active-class="current">
            <a>文章</a>
          </router-link>
          <router-link to="/question" tag="li" active-class="current">
            <a>问答</a>
          </router-link> -->
        </ul>
        <!-- / nav -->
        <ul class="h-r-login">
          <li v-if="userInfo" id="is-login-two" class="h-r-user">
            <a href="/ucenter" title>
              <img
                :src="userInfo.avatar"
                width="30"
                height="30"
                class="vam picImg"
                alt>
              <span id="userName" class="vam disIb">{{ userInfo.nickname }}</span>
            </a>
            <a href="javascript:void(0);" title="退出" class="ml5" @click="logout">退出</a>
          </li>
          <li v-else id="no-login">
            <a href="/login" title="登录">
              <em class="icon18 login-icon">&nbsp;</em>
              <span class="vam ml5">登录</span>
            </a>
            |
            <a href="/register" title="注册">
              <span class="vam ml5">注册</span>
            </a>
          </li>
          <!-- <li v-if="userInfo" id="is-login-one" class="mr10">
            <a id="headerMsgCountId" href="#" title="消息">
              <em class="icon18 news-icon">&nbsp;</em>
            </a>
            <q class="red-point">&nbsp;</q>
          </li> -->
          <!-- /登录显示第1 li；未登录后显示第2 li -->
        </ul>
        <aside class="h-r-search">
          <form action="#" method="post">
            <label class="h-r-s-box">
              <input type="text" placeholder="搜索课程" name="queryCourse.courseName" value>
              <button type="submit" class="s-btn">
                <em class="icon18">&nbsp;</em>
              </button>
            </label>
          </form>
        </aside>
      </div>
      <aside class="mw-nav-btn">
        <div class="mw-nav-icon"/>
      </aside>
      <div class="clear"/>
    </section>
  </header>
  <!-- /公共头 -->
</template>

<script>
import loginApi from '~/api/login'
import cookie from 'js-cookie'

export default {

  data() {
    return {
      userInfo: null,
      token: ''
    }
  },
  created() {
    this.token = this.$route.query.token
    if (this.token) { // 判断路径是否有token值
      this.wxLogin()
    }
    this.getUserInfo()
  },
  methods: {
    wxLogin() {
      // 把token值放到cookie里面
      cookie.set('guli_jwt_token', this.token, { domain: 'localhost' })
      // 调用接口，根据token值获取用户信息
      loginApi.getLoginInfo()
        .then(response => {
          this.userInfo = response.data.userInfo
          cookie.set('guli_ucenter', this.userInfo, { domain: 'localhost' })
        })
    },
    getUserInfo() {
      // 如果token存在，则从cookie获取用户信息
      const userStr = cookie.get('guli_ucenter')
      if (userStr) {
        this.userInfo = JSON.parse(userStr)
      }
    },
    logout() {
      // 清除cookie
      cookie.set('guli_jwt_token', '', { domain: 'localhost' })
      cookie.set('guli_ucenter', '', { domain: 'localhost' })
      // 跳转到网站的首页面
      window.location.href = '/'
    }
  }
}
</script>
