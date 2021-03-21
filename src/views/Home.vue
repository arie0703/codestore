<template>
  <div class="home" v-if="mounted===true">
    <div class="container" v-if="isLogin===true">
      <div id="code">
        <form v-on:submit.prevent>

          <div class="add">
            <input type="text" id="language" value="" placeholder="言語">
            <button class="btn btn-primary" id="add_button">go</button>
            <button v-on:click="addCode" class="btn btn-primary btn-store">
              Store
            </button>
          </div>

          <div class="row">
            <div class="col-xs-6">
              <textarea id="editor" class="form-control" v-model="newCode"></textarea>
            </div>
            <div class="col-xs-6">
              <div id="result"></div>
            </div>
          </div>
        </form>
      
        
        

      </div>

    </div>

    <div class="not-login" v-if="isLogin===false">
        <p>Loginしてください</p>
    </div>
  </div>
</template>


<style>

.container {
    margin: 10px auto;
    padding: 50px;
}
.add {
    width: 90%;
    margin: 10px auto;
    display: flex;
}

.add .btn {
    margin-right: 10px;
}

.btn-store {
    background-color: green;
    border: green;
}

input[type="text"] {
    margin-right: 10px;
    padding: 0.375rem 0.75rem;
    font-size: 1rem;
    line-height: 1.5;
    color: #495057;
    background-color: #fff;
    background-clip: padding-box;
    border: 1px solid #ced4da;
    border-radius: 0.25rem;
    transition: border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out;
    outline: none;
}

.col-xs-6 {
    width: 40%;
    margin: 10px auto;
}

textarea {
    resize: none;
    height: 450px;
    tab-size: 4;
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
  }
}

</script>
