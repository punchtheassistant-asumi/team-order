# スタバ 目黒アトレ — 注文まとめページ

## セットアップ手順

### 1. Firebase プロジェクト作成

1. https://console.firebase.google.com/ を開く
2. 「プロジェクトを追加」→ 名前を入力（例: `sbux-vote-meguro`）
3. Googleアナリティクスは任意（OFFでOK）

### 2. Firestore Database 作成

1. 左メニュー「Firestore Database」→「データベースを作成」
2. **テストモード** を選択（後でルールを変更可能）
3. ロケーションは `asia-northeast1`（東京）を選択

### 3. ウェブアプリを追加

1. プロジェクトの歯車アイコン → 「プロジェクトの設定」
2. 「マイアプリ」→ `</>` (ウェブ) アイコンをクリック
3. アプリ名を入力（例: `sbux-vote`）→「アプリを登録」
4. **firebaseConfig のオブジェクトをコピー**

### 4. index.html を編集

`index.html` の以下の部分を実際の値に書き換える：

```javascript
const firebaseConfig = {
  apiKey:            "AIzaSy...",
  authDomain:        "sbux-vote-meguro.firebaseapp.com",
  projectId:         "sbux-vote-meguro",
  storageBucket:     "sbux-vote-meguro.appspot.com",
  messagingSenderId: "123456789",
  appId:             "1:123456789:web:abc123"
};
```

### 5. Netlify にデプロイ

**方法A: ドラッグ&ドロップ（最速）**
1. https://app.netlify.com/drop を開く
2. このフォルダ全体をブラウザにドラッグ
3. URLが発行される → 完了！

**方法B: GitHub経由**
1. このフォルダを GitHub リポジトリにプッシュ
2. Netlify で「New site from Git」→ リポジトリを選択
3. Build command: なし、Publish directory: `.`
4. 「Deploy site」

### 6. Firestore セキュリティルール（推奨）

Firebase Console → Firestore → ルール に以下を貼り付け：

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /votes/{voterId} {
      // 誰でも読み書きOK（チーム内限定ツールの場合）
      allow read, write: if true;
    }
  }
}
```

## 使い方

- **通常URL** → 名前入力 → メニュー選択 → 送信
- **集計URL** → `https://あなたのサイト.netlify.app/#admin` または ヘッダーの「📊 集計」ボタン
- **リセット** → 集計画面の最下部「全データをリセット」

## メニューのカスタマイズ

`index.html` の `MENU` オブジェクト（スクリプト内）を編集すればメニューを変更できます：

```javascript
const MENU = {
  drinks: [
    { id: 'd01', emoji: '☕', name: 'ドリップコーヒー', size: 'Tall', price: 440 },
    // ...
  ],
  food: [
    { id: 'f01', emoji: '🥐', name: 'クロワッサン', size: '', price: 390 },
    // ...
  ]
};
```
