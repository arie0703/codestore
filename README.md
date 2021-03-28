## Code Store

<p>Vue.js + Firebaseによるコード保存アプリケーション</p>

## 開発環境・使用ライブラリなど

<li>Vue CLI 4.5.12</li>
<li>npm 6.14.11</li>
<li>node 12.21.0</li>
<li>marked.js</li>
<li>highlight.js</li>
<li>Firebase(Hosting, Auth, Firestore)</li>

## メモ
<li>データ保存規則について</li>
投稿時に生成されるドキュメントIDを事前に取得し、同一の文字列をidとして保存。  
削除時はdeleteCodeの引数にpostのidを指定し、ドキュメントIDと照らし合わせて一致した投稿を削除。  
（削除や編集時にIDを取得する方法として他にいい方法があれば随時変更）  

<li>編集機能</li>
投稿一覧で任意の投稿をクリックすると、エディターに保存したコードが表示されると同時にisEditがtrueとなる。この時「新規投稿」モードから「編集」モードへと切り替わる。  
編集モードに切り替わる時、input[type="hidden"]にクリックした投稿のid（doc_id）が入る。  
postCodeメソッドが発火する際、編集モード（isEdit == true）の時、編集用の処理が走る。  
編集用の処理ではpostCodeの引数として指定された投稿IDで編集対象のデータを探し、update処理をする。

## やりたいこと
<li>タイトルをつける</li>
<li>コード記述画面のUI設計</li>


