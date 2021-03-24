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

## やりたいこと
<li>編集機能</li>
<li>タイトルをつける</li>
<li>コード記述画面のUI設計</li>


