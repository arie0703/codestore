## Code Store

<p>Vue.js + Firebaseによるプロジェクト  </p>

## 使用技術・ライブラリなど

<li>Vue.js</li>
<li>marked.js</li>
<li>highlight.js</li>
<li>Firebase(Hosting, Auth, Firestore)</li>

## メモ
<li>データ保存規則について</li>
docにユーザーID + 投稿ナンバーを指定し、保存。削除の際もdocの引数として、ユーザーID + 投稿ナンバーを指定する。（テストの段階で試行錯誤&方法変えるかも）  
docの命名規則変更時はaddCodeとdeleteCodeのメソッドを書き換え、同じものに設定する。  


