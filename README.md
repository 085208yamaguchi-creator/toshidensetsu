# toshidensetsu

- `index.html` … 都市伝説相性診断（9問で6タイプに分類）
- `matching.html` … 都市伝説マッチング（好きなジャンルが近い人にイイね → お互いイイねでマッチ → Threads ID が見られる）

## マッチングの動かし方

`firebase-config.js` が `null` のままだと **デモモード** で動きます。
架空のサンプル会員8人が出てきて、イイね→マッチの流れをひとりで試せます（データはその端末の中だけ）。

みんなでマッチングするには、無料の Firebase を使います。

1. https://console.firebase.google.com でプロジェクトを作る
2. 「Authentication」→「ログイン方法」で **匿名** を有効にする
3. 「Firestore Database」を作成する（本番モードでOK）
4. Firestore の「ルール」タブに `firestore.rules` の中身を貼って公開する
5. 「プロジェクトの設定」→「マイアプリ」でウェブアプリを追加し、表示された `firebaseConfig` を `firebase-config.js` に貼る
6. GitHub Pages などで公開する（Authentication → 設定 → 承認済みドメイン に公開先のドメインを追加）

### しくみ

| コレクション | 中身 | 読める人 |
| --- | --- | --- |
| `profiles/{uid}` | ニックネーム・タイプ・好きなジャンル・ひとこと | ログイン中の全員 |
| `likes/{送った人}_{受けた人}` | イイね | 当事者2人だけ |
| `contacts/{uid}` | Threads ID | 本人とマッチした相手だけ |

ログインは匿名認証なので、ブラウザのデータを消すと別人扱いになります。
