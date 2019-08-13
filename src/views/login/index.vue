<template>
  <div class="login">
    <div class="universe">
      <div class="sun" />
      <div class="moon" />
    </div>
    <div class="login-container">
      <el-form ref="loginForm" :model="loginForm" class="login-form">
        <i class="el-icon-user" />
        <input v-model="loginForm.username" required type="text">
        <label alt="请输入名称" placeholder="名称" />

        <i class="el-icon-key" />
        <input v-model="loginForm.password" required :type="passwordType">
        <label alt="请输入密码" placeholder="密码" />
        <i v-if="passwordType=='password'" class="el-icon-lock hidePwd" @click="showPwd" />
        <i v-if="passwordType!='password'" class="el-icon-unlock hidePwd" @click="showPwd" />
        <el-button class="subtn" round @click="handleLogin">登录</el-button>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Login',
  data() {
    return {
      loginForm: {
        username: 'admin',
        password: '111111'
      },
      loading: false,
      passwordType: 'password',
      redirect: undefined
    }
  },
  watch: {
    $route: {
      handler: function(route) {
        this.redirect = route.query && route.query.redirect
      },
      immediate: true
    }
  },
  methods: {
    showPwd() {
      if (this.passwordType === 'password') {
        this.passwordType = ''
      } else {
        this.passwordType = 'password'
      }
      this.$nextTick(() => {
        this.$refs.password.focus()
      })
    },
    handleLogin() {
      this.$refs.loginForm.validate(valid => {
        if (valid) {
          this.loading = true
          this.$store
            .dispatch('user/login', this.loginForm)
            .then(() => {
              this.$router.push({ path: this.redirect || '/' })
              this.loading = false
            })
            .catch(() => {
              this.loading = false
            })
        } else {
          console.log('error submit!!')
          return false
        }
      })
    }
  }
}
</script>

<style lang="scss" scoped>
@import url("./input.css");
@import url("./solar.css");
$bg: #2d3a4b;
$dark_gray: #889aa4;
$light_gray: #eee;

.login-container {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -35%);
  z-index: 999;

  .login-form {
    font-size: 16px;
    max-width: 100%;
    margin: 10px auto;

    i {
      font-size: 30px;
      margin-right: 15px;
      font-weight: bold;
      color: #000;
    }

    .hidePwd {
      font-size: 26px;
      position: relative;
      left: 320px;
      top: -100px;
    }

    .hidePwd:hover {
      color: #0f0;
    }

    .subtn {
      position: relative;
      left: 30%;
      margin-top: 20px;
      // transform: translate(-50%, 0);
    }
  }

  .tips {
    font-size: 14px;
    color: #fff;
    margin-bottom: 10px;

    span {
      &:first-of-type {
        margin-right: 16px;
      }
    }
  }

  .svg-container {
    padding: 6px 5px 6px 15px;
    color: $dark_gray;
    vertical-align: middle;
    width: 30px;
    display: inline-block;
  }

  .title-container {
    position: relative;

    .title {
      font-size: 26px;
      color: $light_gray;
      margin: 0px auto 40px auto;
      text-align: center;
      font-weight: bold;
    }
  }

  .show-pwd {
    position: absolute;
    right: 10px;
    top: 7px;
    font-size: 16px;
    color: $dark_gray;
    cursor: pointer;
    user-select: none;
  }
}
</style>
