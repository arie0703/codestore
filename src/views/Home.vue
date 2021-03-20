<template>
  <div class="home">
    <div class="container">
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
// import $ from 'jquery'

// export default {
//   name: 'home',
//   data() {
//     return{
//       newCode: '',
//       code_list: [],
//       modal_content: '',
//       output_code: ''
//     }
//   },
//   methods: {
//   	addCode: function() {
//       if(this.newCode == "") return;
        
//         var num = this.code_list.length + 1;
//     	  var code = {
//             title: num,
//             content: this.newCode
//         };

//         this.code_list.push(code);
//         this.newCode = "";
//         $('#result').html("");
//     },
//     showCode: function(index) {

//         this.newCode = this.code_list[index].content;
        
//         //メソッド発火時にmarkedも発動するように設定！
//         var src = this.newCode;
//         var html = marked(src);
//         $('#result').html(html);
//         $('pre code').each(function(i, block) {
//             hljs.highlightBlock(block);
//         });
        
//     },
//     closeCode : function () {
//         this.$modal.hide('hello-world');
//     },
//   },
// }
</script>
