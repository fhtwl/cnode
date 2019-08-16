<template>
  <div class="head">
      <a href="/" class="logo"><img src="../assets/images/logo.svg" alt=""></a>
      <div class="denglu" @click="login()">登录</div>
  </div>
</template>

<script>
import axios from 'axios'
import { constants } from 'crypto';
export default {
  name: 'Head',
  methods: {
    login() {
        axios({
            url:"https://cnodejs.org/api/v1/user/alsotang",
            method:"post",
            data:{accessToken:"cb56b1c6-8fb1-492f-9c44-0b0c9f59c930"}
        })
        .then(response => {
            console.log(response)
        })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
    .head {
        width:100%;
        background:#444;
        display: flex;
        justify-content: space-between;
        align-items: center;
        .logo {
            width:1.2rem;
            display:block;
            font-size:0;
            margin-left:0.3rem;
            padding:0.1rem 0;
            img {
              width:100%;
            }
        }
        .denglu {
          color:#fff;
          margin-right:0.3rem;
        }
    }
</style>
