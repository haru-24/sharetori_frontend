

# シェアトリ

[https://purafuru.web.app](https://purafuru.web.app/)

## 1.概要
自分の故郷や旅先などよく行くの「お店」,「景色」,「建物」などをシェアして、投稿した記事を見た人が観光する際に,
新たに旅先の名産を発掘することができます。

### どんな課題を解決したいか


観光業
- コロナ禍で衰退していた観光業を、このアプリを利用している人がさまざまなところに行くことにより活力を取り戻してほしい

投稿者
- 自分の故郷にはこんなものがあるのだと自慢した気持ちになれる
- 良いものを皆んなと共有できる
- 集客に繋がったら何かプレゼントがあるといいかも

閲覧者
- 観光に来たけれど、観光ガイドに乗っているお店は前回もいったしな、行ったことないお店に行ってみたい！このアプリで探すことができる
- 全国的に有名なものだけではなく、地元で有名（B級グルメみたいな）ものを探して行くことが出来る

### トップページ
<img width="1429" alt="スクリーンショット 2021-12-09 16 00 38" src="https://user-images.githubusercontent.com/88644715/145349515-4ad1d6b8-5dae-4db9-9a75-bcb43b9ba411.png">

## 2.使用技術
フロントエンド
* Nuxt.js 2.15
* TypeScript

バックエンド
* Node.js 16.13
* express 4.17
 
DB
* Heroku Posgres

インフラ
* Firebase Hosting
* Firebase Storage
* Heroku

自動デプロイ
* GitHub Actions

## 3.設計
### 画面設計
* Figmaで画面遷移図とワイヤーフレームを作成しました。
<img width="942" alt="スクリーンショット 2021-12-09 16 23 27" src="https://user-images.githubusercontent.com/88644715/145352100-d828f323-f174-4a7a-959b-cba884230773.png">

### ER図
![スクリーンショット 2021-12-09 16 44 45](https://user-images.githubusercontent.com/88644715/145354834-b8d4eb21-8662-4672-bd92-ea59b31f6e79.png)

### インフラ構成図
![スクリーンショット 2021-12-12 14 37 21](https://user-images.githubusercontent.com/88644715/145701718-544e3525-c383-4b0d-a064-eedbd879f4ee.png)


## 3.機能一覧
* トップページ
  * 新着投稿10件をカルーセルで表示

* 検索画面
  * 都道府県とジャンルで選択
  * 10件ごとにページネーション
  * 新着順とお気に入り数順に並び替え
 
* 記事画面
  * 投稿した情報、画像を閲覧
  * ログイン時、お気に入り登録機能
  * ログイン時,コメント機能
 
* 投稿画面
  * 画像投稿機能
   　

* マイページ
  * 投稿履歴閲覧機能
  * 投稿編集機能
  * 投稿削除
  * お気に入りした記事を閲覧 

* ユーザー登録/ログイン機能
  
* レスポンシブ
　　   



<img width="376" alt="スクリーンショット 2021-12-09 17 58 17" src="https://user-images.githubusercontent.com/88644715/145365575-b2742842-43fb-49ac-b8cb-df1b4dd02a3e.png">


<img width="374" alt="スクリーンショット 2021-12-09 17 59 42" src="https://user-images.githubusercontent.com/88644715/145365585-04c5a065-6962-41a4-84c9-0663f2e9cd88.png">








