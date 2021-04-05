<template>
  <div class="login">
    <div class="signup">

      <p>ログイン / 新規登録</p>
      <input type="email" class="form-control" placeholder="E-mail" v-model="mailaddress"/>
      <input type="password" class="form-control" placeholder="Password" v-model="password"/>

      <button @click="signIn" class="btn btn-primary">Sign In</button>
      <button @click="signUp" class="btn btn-primary register">Register</button>
    </div>

  </div>

</template>

<style scoped>

input {
  display: block;
  margin: 10px auto;
  width: 300px;
}
.login {
  text-align: center;
  margin: 0 auto;
  background: #dedede;
  width: 60%;
  padding: 30px;
  border-radius: 10px;
}
button {
  display: block;
  margin: 10px auto;
}

.register {
  background-color: orange;
  border: orange;
}
</style>

<script>

import firebase from "firebase";

export default {
  name: 'Login',
  data () {
    return {
      mailaddress: '',
      password: ''
    }
  },
  methods: {
    signUp: function () {
      firebase.auth().createUserWithEmailAndPassword(this.mailaddress, this.password)
      .then(
        alert('Success'),
        this.$router.push('/')
      )
    },
    signIn: function () {
      firebase.auth().signInWithEmailAndPassword(this.mailaddress, this.password).then(
        (data) => {
          console.log('Success!' + data.user.email);
          this.$router.push('/')
          
          
        },
        err => {
          alert("ログインに失敗しました・・。");
          console.log(err)
        }
      )
    }
  }
}
</script>


