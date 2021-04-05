<template>
  <div class="home" v-if="mounted===true">
    <div class="container" v-if="isLogin===true">
      <div id="code">
        <form v-on:submit.prevent>

          <div class="menu-bar">
            <input type="text" class="title" v-model="title" placeholder="タイトル">
            <button v-on:click="postCode(postId)" class="btn btn-primary btn-store">
              Store
            </button>
            <button v-on:click="clear" class="btn btn-danger">
              Clear
            </button>
            <p v-if="isEdit">編集モード</p>
            <p v-if="!isEdit">新規投稿</p>
          </div>

          <div class="menu-bar-2">
            <input type="text" class="lang" id="language" value="" placeholder="言語">
            <button class="btn btn-primary btn-lang" id="add_button">go</button>
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
            <span class="post" v-on:click="showCode(post.title, post.code, post.id)">{{post.title}}</span>
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
      title: "",
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
          title: this.title,
          code: this.newCode,
          user_id: currentUser,
          created_at: new Date
        }).then(
          console.log(doc)
        )
          
        //入力フォームクリア
        this.clear();
        //reset
        this.reloadPosts();

      } else if (this.isEdit == true) { //編集モード

        db.collection("posts").doc(postId).update({
          title: this.title,
          code: this.newCode,
        }).then(
          console.log("edited!")
        )

        this.clear();
        setTimeout(this.reloadPosts,200);
      }
    },
    showCode: function(title, code ,id) {

        this.newCode = code;
        this.title = title;
        
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
      this.title = "";
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
    margin: 0 auto;
    padding: 0px 50px;
    text-align: left;
}
.menu-bar {
    width: 90%;
    margin: 0px auto;
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

.menu-bar .title {
  width: 300px;
}
.menu-bar-2 {
  width: 90%;
  height: 50px;
  margin: 0 auto;
  display: flex;
  background-color: #e4e4e4;
  padding: 10px;
}

.menu-bar-2 input[type="text"]{
  height:30px;
}

.btn-lang {
  height: 30px;
  line-height: 10px;
  background: #555;
  border: #555;
}

.btn-lang:hover {
  background: #333;
  border: #333;
}

.btn-store {
  background-color: green;
  border: green;
}

.btn-store:hover {
  background-color: darkgreen;
  border: darkgreen;
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
