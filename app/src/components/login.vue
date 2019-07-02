<template>
  <div id="login">
    <div class="back">
      <img src="../../static/images/back.png" alt @click="back">
    </div>

    <form action>
      <h1>login</h1>
      <input type="text" placeholder="user name" v-model="username">
      <input type="password" placeholder="password" v-model="password">
      <input type="button" class="btn" value="login" @click="login">
      <input type="button" class="btn" value="register" @click="toRegister">
    </form>
  </div>
</template>

<script>
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
    login() {
      var users = JSON.parse(localStorage.getItem("users")) || [];
      for (let i = 0; i < users.length; i++) {
        if (this.username === users[i].username) {
          if (this.password === users[i].password) {
            this.$router.push({
              path: "/index",
              query: {
                as: users[i].as,
                cp: users[i].cp,
                signature: users[i].signature
              }
            });
          }
        }
      }
    },
    toRegister() {
      this.$router.push({
        path: "/register"
      });
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

#login {
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
</style>
