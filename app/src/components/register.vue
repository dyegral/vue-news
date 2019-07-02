<template>
  <div id="register">
    <div class="back">
      <img src="../../static/images/back.png" alt @click="back">
    </div>
    <form action>
      <h1>register</h1>
      <input type="text" placeholder="user name" v-model="username">
      <input type="password" placeholder="password" v-model="password">
      <input type="button" class="btn" value="register" @click="register">
      <p @click="toLogin">Already Have one?</p>
    </form>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      username: "",
      password: ""
    };
  },
  methods: {
    back() {
      this.$router.push({
        path: "/"
      });
    },
    toLogin() {
      this.$router.push({
        path: "/login"
      });
    },
    register() {
      if (this.username === "" || this.password === "") {
        return;
      }

      var users = JSON.parse(localStorage.getItem("users")) || [];
            for (let i = 0; i < users.length; i++) {
              if(users[i].username === this.username) {
                alert('已有该用户名');
                return;
              }
            }

      let that = this;
      //请求3个加密信息
      axios
        .get("http://zzzia.net:8080/toutiao/init")
        .then(function(res) {
          //返回的数据都在res.data里面
          // console.log(res.data);
          if (res.data.status == 200) {
            
            var user = {};
            user.username = that.username;
            user.password = that.password;
            user.as = res.data.info.as;
            user.cp = res.data.info.cp;
            user.signature = res.data.info._signature;
            users.push(user);
            localStorage.setItem("users", JSON.stringify(users));

            that.$router.push({
              path: "/login"
            });
          }
        })
        .catch(function() {});
    }
  }
};
</script>

<style>
/* body {
  background-color: skyblue;
} */

.back {
  width: 1.5rem;
  height: 1.5rem;
  margin: 0.5rem;
}

.back img {
  width: 1.5rem;
  height: 1.5rem;
}

#register {
  text-align: center;
}

form {
  width: 80%;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

input {
  /* display: block; */
  box-sizing: border-box;
  outline: none;
  border: none;
  width: 80%;
  margin: 0.7rem;
  padding: 0.7rem 1rem;

  border: 1px solid #ccc;
  border-radius: 0.5rem;
  text-align: center;
}

.btn {
  color: #333;
  font-weight: 600;
  background-color: #ccc;
}

p {
  color: #333;
}
</style>
