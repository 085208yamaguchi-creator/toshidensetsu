# toshidensetsu

- `index.html` … 都市伝説相性診断（9問で6タイプに分類）
- `matching.html` … 都市伝説マッチング（趣味友さがし。好きなジャンルが近い人にイイね → お互いイイねでマッチ → 相手のSNSが見られる）
- `terms.html` … 利用規約・プライバシーポリシー（【運営者名】【お問い合わせ先】を書きかえてから公開）

## マッチングの動かし方

`firebase-config.js` が `null` のままだと **デモモード** で動きます。
架空のサンプル会員8人が出てきて、イイね→マッチの流れをひとりで試せます（データはその端末の中だけ）。

みんなでマッチングするには、無料の Firebase を使います。

1. https://console.firebase.google.com でプロジェクトを作る
2. 「Authentication」→「ログイン方法」で **Google** と **メール/パスワード** を有効にする
3. 「Firestore Database」を作成する（本番モードでOK）
4. Firestore の「ルール」タブに `firestore.rules` の中身を貼って公開する
5. 「プロジェクトの設定」→「マイアプリ」でウェブアプリを追加し、表示された `firebaseConfig` を `firebase-config.js` に貼る
6. GitHub Pages などで公開する（Authentication → 設定 → 承認済みドメイン に公開先のドメインを追加）

### しくみ

| コレクション | 中身 | 読める人 |
| --- | --- | --- |
| `profiles/{uid}` | ニックネーム・タイプ・好きなジャンル・ひとこと | ログイン中の全員 |
| `likes/{送った人}_{受けた人}` | イイね | 当事者2人だけ |
| `contacts/{uid}` | SNSのユーザー名（Threads・X・Instagram・Discord） | 本人とマッチした相手だけ |
| `blocks/{uid}` | ブロックした相手 | 本人だけ |
| `reports/{id}` | 通報 | 誰も読めない（運営が Firebase コンソールで確認） |

通報は Firebase コンソールの Firestore →「reports」で確認できます。違反者のプロフィールはコンソールから削除してください。
