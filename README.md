# [mom-git-my-site](https://suidaikitaka.github.io/mom-git-my-site/)

髙橋 三保子のポートフォリオサイトです。

## 特徴

- **HOME** - ウェルカムページ
- **PROFILE/CONTACT** - プロフィール・連絡先情報
- **WORKS** - ライターとして手がけた仕事の紹介

## 実績管理機能

WORKSページの実績は `data.json` ファイルで管理されています。

### 管理画面へのアクセス

ページを開いた状態で、**Ctrl + Shift + M** を同時に押すと、管理画面が表示されます。

### 実績を追加する手順

1. **Ctrl + Shift + M** で管理画面を開く
2. 以下の情報を入力：
   - **記事タイトル・企業名** - 例：「【株式会社スタディスト】営業経験を活かして...」
   - **リンクするURL** - 記事のリンク
   - **パスワード（GitHubトークン）** - 更新に必要な認証情報
3. 「この内容でサイトを更新」をクリック

## デザイン

参考URL: [hana-tsuki.themedia.jp](https://hana-tsuki.themedia.jp/)

カラースキーム：
- メイン色：#473c34 (ブラウン)
- 背景色：#f1f0e5 (ベージュ)
- リンク色：#8d8148 (ゴールドブラウン)

## ローカルで実行

```bash
python3 -m http.server 8000
```

その後、ブラウザで `http://localhost:8000` にアクセスしてください。

## GitHubに同期

```bash
git add .
git commit -m "変更内容の説明"
git push origin main
```
