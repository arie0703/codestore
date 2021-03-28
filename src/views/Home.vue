<template>
  <div class="home" v-if="mounted===true">
    <div class="container" v-if="isLogin===true">
      <div id="code">
        <form v-on:submit.prevent>

          <div class="menu-bar">
            <input type="text" id="language" value="" placeholder="言語">
            <button class="btn btn-primary" id="add_button">go</button>
            <button v-on:click="postCode(postId)" class="btn btn-primary btn-store">
              Store
            </button>
            <button v-on:click="clear" class="btn btn-danger">
              Clear
            </button>
            <p v-if="isEdit">編集モード</p>
            <p v-if="!isEdit">新規投稿</p>
          </div>

          <div class="row">
            <div class="col-xs-6">
              <input type="hidden" v-model="postId">
              <textarea id="editor" class="form-control" v-model="newCode"></textarea>
            </div>
            <div class="col-xs-6">
              <div id="result"></div>
            </div>
          </div>
        </form>
        
        <ul v-for="(post, index) in posts" v-bind:key="index">
          <li>
            <span class="post" v-on:click="showCode(post.code, post.id)">{{post.title}}</span>
            <span class="delete" v-on:click="deleteCode(post.id)">Delete</span>
          </li>
        </ul>
      </div>

    </div>

    <div class="not-login" v-if="isLogin===false">
        <p>Loginしてください</p>
    </div>
  </div>
</template>

<script>
import firebase from "firebase";
import {db} from '../main.js';
import $ from 'jquery';
import marked from 'marked';
import hljs from 'highlightjs';

export default {
  data () {
    return {
      mounted: false,
      isLogin: false,
      isEdit: false,
      posts: [],
      newCode: "",
      postId: ""
    }
  },
  mounted: function() {
    firebase.auth().onAuthStateChanged((user) => {
      if (user) {
        
        // postsインスタンスに投稿データ代入
        db.collection("posts")
        .where("user_id","==",firebase.auth().currentUser.email)
        .get()
        .then((querySnapshot) => {
          querySnapshot.forEach((doc) => {
              this.posts.push(doc.data())
          });
        });

        //mounted判定 → viewの読み込み
        this.isLogin = true
        this.mounted = true


        //marked.jsの設定ファイルをここで読み込ませる
        var el = document.createElement("script");
        el.src = "scripts/config.js";
        document.body.appendChild(el);

        // ...
      } else {
        // User is signed out
        // ...
        this.isLogin = false
        this.mounted = true
      }
    });
  },
  methods: {
    postCode: function(postId) {
      if(this.newCode == "") return;

      if (this.isEdit == false) { //新規投稿
        var currentUser = firebase.auth().currentUser.email
        //ドキュメントIDを取得・投稿IDとして利用
        var doc = db.collection("posts").doc().id
        
        db.collection("posts").doc(doc).set({
          id: doc,
          title: this.posts.length,
          code: this.newCode,
          user_id: currentUser,
          created_at: new Date
        }).then(
          console.log(doc)
        )
          
        this.newCode = "";
        $('#result').html("");

        //reset
        this.reloadPosts();

      } else if (this.isEdit == true) { //編集モード

        db.collection("posts").doc(postId).update({
          title: this.posts.length,
          code: this.newCode,
        }).then(
          console.log("edited!")
        )

        this.newCode = "";
        $('#result').html("");
        this.reloadPosts();
        this.postId = "";
        this.isEdit = false;
      }
    },
    showCode: function(code ,id) {

        this.newCode = code;
        
        //メソッド発火時にmarkedも発動するように設定！
        var src = this.newCode;
        var html = marked(src);
        $('#result').html(html);
        $('pre code').each(function(i, block) {
            hljs.highlightBlock(block);
        });

        this.isEdit = true;
        this.postId = id;
        
    },
    deleteCode: function(doc) {

      db.collection("posts").doc(doc).delete().then(() => {
        this.reloadPosts()
        alert("deleted.")
      });
    },
    reloadPosts: function() {
      this.posts = [];
      db.collection("posts")
        .where("user_id","==",firebase.auth().currentUser.email)
        .get()
        .then((querySnapshot) => {
          querySnapshot.forEach((doc) => {
              this.posts.push(doc.data())
          });
      });
    },
    clear: function() {
      this.newCode = "";
      $('#result').html("");
      this.postId = "";
      this.isEdit = false;
    }
  }
}

</script>


<style>

.container {
    margin: 10px auto;
    padding: 50px;
    text-align: left;
}
.menu-bar {
    width: 90%;
    margin: 10px auto;
    display: flex;
    background-color: #e4e4e4;
    padding: 10px;
}

.menu-bar .btn {
    margin-right: 10px;
}

.menu-bar p {
  margin: auto 0px;
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

li {
  list-style: none;
}

.delete {
  color: #ec1818;
  padding-left: 15px;
}

</style>
