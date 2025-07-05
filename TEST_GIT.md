# Git 使い方

## 目次
1. [Gitの基本概念](#gitの基本概念)
2. [初期設定](#初期設定)
3. [基本的なコマンド](#基本的なコマンド)
4. [ブランチ操作](#ブランチ操作)
5. [リモートリポジトリ](#リモートリポジトリ)
6. [よく使うコマンド](#よく使うコマンド)
7. [トラブルシューティング](#トラブルシューティング)

---

## Gitの基本概念

### ワークスペース、ステージング、リポジトリ
```bash
# ワークスペース → ステージング → リポジトリ
git add .          # ワークスペース → ステージング
git commit -m ""   # ステージング → リポジトリ
```

### ファイルの状態
- **Untracked**: Gitが追跡していないファイル
- **Modified**: 変更されたファイル
- **Staged**: ステージングエリアに追加されたファイル
- **Committed**: リポジトリに保存されたファイル

---

## 初期設定

### 1. ユーザー情報の設定
```bash
# グローバル設定（全リポジトリで使用）
git config --global user.name "あなたの名前"
git config --global user.email "あなたのメールアドレス"

# 現在のリポジトリのみの設定
git config user.name "あなたの名前"
git config user.email "あなたのメールアドレス"
```

### 2. 設定の確認
```bash
# 全設定を表示
git config --list

# 特定の設定を確認
git config user.name
git config user.email
```

### 3. リポジトリの初期化
```bash
# 新しいリポジトリを作成
git init

# 既存のリポジトリをクローン
git clone https://github.com/username/repository.git
```

---

## 基本的なコマンド

### 1. ファイルの追加とコミット
```bash
# 特定のファイルをステージングに追加
git add filename.txt

# 全てのファイルをステージングに追加
git add .

# 特定の拡張子のファイルを追加
git add *.js

# 変更をコミット
git commit -m "コミットメッセージ"

# ステージングとコミットを同時に行う
git commit -am "コミットメッセージ"
```

### 2. 状態の確認
```bash
# 現在の状態を確認
git status

# 変更内容を確認
git diff

# ステージングされた変更を確認
git diff --staged

# コミット履歴を確認
git log

# 簡潔なログを表示
git log --oneline
```

### 3. 変更の取り消し
```bash
# ステージングを取り消し
git reset HEAD filename.txt

# ファイルの変更を取り消し
git checkout -- filename.txt

# 最新のコミットを取り消し（コミットは残る）
git reset --soft HEAD~1

# 最新のコミットを完全に削除
git reset --hard HEAD~1
```

---

## ブランチ操作

### 1. 
```bash
# 新しいブランチを作成
git branch feature-branch

# ブランチを作成して切り替え
git checkout -b feature-branch

# ブランチの切り替え
git checkout feature-branch

# 最新のブランチ切り替え方法
git switch feature-branch
git switch -c new-branch
```

### 2. ブランチの管理
```bash
# ブランチ一覧を表示
git branch

# リモートブランチも含めて表示
git branch -a

# ブランチを削除
git branch -d branch-name

# 強制削除
git branch -D branch-name
```

### 3. マージ
```bash
# 現在のブランチに別のブランチをマージ
git merge feature-branch

# マージコンフリクトが発生した場合
# 1. コンフリクトを解決
# 2. ファイルをステージングに追加
git add .
# 3. マージを完了
git commit
```

### 4. リベース
```bash
# リベース（履歴をきれいに保つ）
git rebase main
```

---

## リモートリポジトリ

### 1. リモートの追加と確認
```bash
# リモートリポジトリを追加
git remote add origin https://github.com/username/repository.git

# リモート一覧を確認
git remote -v

# リモートを削除
git remote remove origin
```

### 2. プッシュとプル
```bash
# 初回プッシュ（ブランチを設定）
git push -u origin main

# 通常のプッシュ
git push origin main

# リモートから最新情報を取得
git fetch origin

# フェッチとマージを同時に行う
git pull origin main

# リモートブランチをローカルに作成
git checkout -b local-branch origin/remote-branch
```

### 3. フォークとプルリクエスト
```bash
# フォークしたリポジトリを追加
git remote add upstream https://github.com/original/repository.git

# アップストリームから最新を取得
git fetch upstream
git merge upstream/main
```

---

## よく使うコマンド

### 1. 日常的なワークフロー
```bash
# 1. 最新の変更を取得
git pull origin main

# 2. 新しいブランチを作成
git checkout -b feature/new-feature

# 3. 変更を加える
# ... ファイルを編集 ...

# 4. 変更をステージング
git add .

# 5. コミット
git commit -m "Add new feature"

# 6. プッシュ
git push origin feature/new-feature
```

### 2. 便利なコマンド
```bash
# 変更を一時的に保存
git stash

# 保存した変更を復元
git stash pop

# 保存した変更一覧
git stash list

# 特定のコミットに戻る
git checkout commit-hash

# タグを作成
git tag v1.0.0

# タグをプッシュ
git push origin v1.0.0
```

### 3. 履歴の確認
```bash
# グラフィカルなログ表示
git log --graph --oneline --all

# 特定のファイルの変更履歴
git log --follow filename.txt

# 特定の作者のコミット
git log --author="username"

# 特定の日付範囲のコミット
git log --since="2023-01-01" --until="2023-12-31"
```

---

## トラブルシューティング

### 1. よくある問題と解決方法

#### コミットメッセージを間違えた
```bash
# 最新のコミットメッセージを変更
git commit --amend -m "新しいメッセージ"
```

#### 間違ったファイルをコミットした
```bash
# 最新のコミットからファイルを削除
git reset HEAD~1 filename.txt
git checkout -- filename.txt
```

#### プッシュが拒否された
```bash
# リモートの変更を先に取得
git pull origin main

# または、強制プッシュ（注意して使用）
git push --force origin main
```

#### マージコンフリクト
```bash
# 1. コンフリクトファイルを確認
git status

# 2. コンフリクトを解決

# 3. 解決したファイルを追加
git add .

# 4. マージを完了
git commit
```

### 2. 緊急時の復旧
```bash
# 全ての変更を破棄
git reset --hard HEAD

# 特定のファイルを最新のコミット状態に戻す
git checkout HEAD -- filename.txt

# リモートの状態に強制リセット
git fetch origin
git reset --hard origin/main
```

---

## ベストプラクティス

### 1. コミットメッセージ
```bash
# 良い例
git commit -m "feat: ユーザー認証機能を追加"
git commit -m "fix: ログインエラーを修正"
git commit -m "docs: READMEを更新"

# 悪い例
git commit -m "修正"
git commit -m "更新"
```

### 2. ブランチ命名規則
```bash
# 機能開発
feature/user-authentication
feature/payment-integration

# バグ修正
fix/login-error
fix/database-connection

# ホットフィックス
hotfix/security-patch
```

### 3. 定期的なメンテナンス
```bash
# 不要なブランチを削除
git branch --merged | grep -v "\*" | xargs -n 1 git branch -d

# リモートの削除されたブランチを削除
git remote prune origin

# リポジトリをクリーンアップ
git gc
```

---

## 参考リンク
- [Git公式ドキュメント](https://git-scm.com/doc)
- [GitHub公式ガイド](https://guides.github.com/)
- [Pro Git Book](https://git-scm.com/book/ja/v2)

---

*このガイドは継続的に更新されます。最新の情報については公式ドキュメントを参照してください。* 