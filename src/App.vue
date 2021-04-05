<template>
  <div id="nav" v-if="mounted">
    <div v-if="isLogin===false">
    <router-link to="/">Home</router-link> |
    <router-link to="/login">Sign In</router-link> |
    </div>
    <div v-if="isLogin===true">
    <router-link to="/">Home</router-link> |
    <span @click="signOut">Sign Out</span> |
    </div>
  </div>
  <router-view/>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  text-align: center;
  background-color: #efefef;
}
#nav {
  padding: 30px;
  margin: 0 auto;
  text-align: center;
}

#nav a {
  font-weight: bold;
  color: #42b983;
}

#nav a.router-link-exact-active {
  color: #2c3e50;
}

span {
  cursor: pointer;
  font-weight: bold;
  color: #42b983;
}

span:hover {
  text-decoration: underline;
}


</style>


<script>
import firebase from "firebase";
export default {
  data () {
    return {
      mounted: false,
      isLogin: false
    }
  },
  mounted: function() {
    firebase.auth().onAuthStateChanged((user) => {
      if (user) {
        // User is signed in, see docs for a list of available properties
        // https://firebase.google.com/docs/reference/js/firebase.User
        this.isLogin = true
        this.mounted = true
        // ...
      } else {
        // User is signed out
        // ...
        this.isLogin = false
        this.mounted = true
      }
    });
  },
  constructor() {
    this.user = firebase.auth().currentUser.uid
  },
  methods: {
    signOut: function() {
      firebase.auth().signOut().then(()=>{
        alert("ログアウトしました");
        this.$router.push('/')
        location.reload();
      })
      .catch( (error)=>{
        console.log(`ログアウト時にエラーが発生しました (${error})`);
      });
    }
  }
}

</script>
