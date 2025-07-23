# GitHubの使い方（TEST用まとめ）

## 1. リポジトリの作成

1. [GitHub](https://github.com/)にアクセスし、ログインします。
2. 右上の「＋」ボタンから「New repository」を選択します。
3. リポジトリ名や説明を入力し、「Create repository」をクリックします。

---

## 2. ローカルリポジトリの初期化

```bash
git init
```

---


## 3. ファイルの追加とコミット aaaa


```bash
git add .
git commit -m "初回コミット"
```

---

## 4. リモートリポジトリの登録

```bash
git remote add origin https://github.com/ユーザー名/リポジトリ名.git
```

---

## 5. ブランチ名の変更（mainに統一）

```bash
git branch -M main
```

---

## 6. GitHubへプッシュ

```bash
git push -u GitHubtest main
```

---

## 7. 変更の反映（push）

```bash
git add .
git commit -m "変更内容"
git push
```

---

## 8. 変更の取得（pull）

```bash
git pull
```

---

## 9. よく使うコマンドまとめ

| コマンド | 説明 |
|----------|------|
| git status | 状態を確認 |
| git log    | コミット履歴を表示 |
| git diff   | 差分を表示 |
| git clone  | リポジトリを複製 |

---

## 10. 注意点

- プッシュ時に認証が必要な場合は、GitHubのPersonal Access Tokenを使うと便利です。
- `.gitignore`ファイルを活用して、不要なファイルを管理しましょう。

---

## 参考リンク

- [GitHub公式ドキュメント](https://docs.github.com/ja)
- [Git公式ドキュメント](https://git-scm.com/book/ja/v2) 