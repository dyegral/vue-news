<template>
  <div id="enter">
    <h1 class="app-name">热度</h1>
    <div class="login-and-register">
      <p @click="toLogin">登录</p>
      <p @click="toRegister">注册</p>
      <p class="btn-enter" @click="toIndex">游客浏览></p>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      as: "",
      cp: "",
      signature: ""
    };
  },
  methods: {
    toLogin() {
      this.$router.push({
        path: "/login"
      });
    },
    toRegister() {
      this.$router.push({
        path: "/register"
      });
    },
    toIndex() {
      let that = this;
      //请求3个加密信息
      axios
        .get("http://zzzia.net:8080/toutiao/init")
        .then(function(res) {
          //返回的数据都在res.data里面
          // console.log(res.data);
          if (res.data.status == 200) {
            that.as = res.data.info.as;
            that.cp = res.data.info.cp;
            that.signature = res.data.info._signature;

            that.$router.push({
              path: "/index",
              query: {
                as: that.as,
                cp: that.cp,
                signature: that.signature
              }
            });
          }
        })
        .catch(function() {});
    }
  }
};
</script>

<style scoped>
#enter {
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  background-color: skyblue;
}

h1 {
  text-shadow: #ff0000 0 0 10px;
  color: white;
  font-size: 60px;
}

.app-name {
  width: 100%;
  text-align: center;
  margin-top: 10rem;
}

.login-and-register {
  position: absolute;
  bottom: 5rem;
  left: 0;
  right: 0;
  text-align: center;
  color: #fff;
  font-weight: 600;
  letter-spacing: 2px;
}
</style>
