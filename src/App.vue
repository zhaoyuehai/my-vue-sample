<template>
  <div id="login">
    <form>
      <label for="username">用户名</label>
      <input type="text" v-model.trim="loginData.username" name="username">
      <br>
      <label for="password">密码</label>
      <input type="password" v-model.trim="loginData.password" name="password">
      <br>
      <input type="button" value="登录" @click="doLogin">
    </form>
  </div>
</template>

<script>
import axios from 'axios';
import { Base64 } from 'js-base64';

export default {
  name: "login",
  data() {
    return {
      loginData: {
        username: "",
        password: ""
      },
      userToken: ""
    };
  },
  methods: {
     doLogin() {
      if (this.loginData.username === "" || this.loginData.password === "") {
        alert("账号或密码不能为空");
      } else {
        var params = new URLSearchParams();
        params.append("username", this.loginData.username);
        params.append("password", Base64.encode(this.loginData.password));//密码进行Base64编码
        axios({
          method: 'post',
          url: 'api/v1/login',
          params: params
        }).then(res => {
          console.log(res.data);
          alert(res.data.message)
          if(res.data.code==="10000"){
            console.log(res.data.data.accessToken);
            this.userToken = 'Bearer ' + res.data.data.accessToken;
           // todo
          }
        }).catch(error => {
          alert('登录失败');
          console.log(error);
        });
      }
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
