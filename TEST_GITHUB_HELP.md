# GitHub 使い方ガイド

## 目次
1. [GitHubとは](#githubとは)
2. [アカウント作成と初期設定](#アカウント作成と初期設定)
3. [リポジトリの作成と管理](#リポジトリの作成と管理)
4. [基本的な操作](#基本的な操作)
5. [ブランチとプルリクエスト](#ブランチとプルリクエスト)
6. [Issuesとプロジェクト管理](#issuesとプロジェクト管理)
7. [GitHub Pages](#github-pages)
8. [GitHub Actions](#github-actions)
9. [セキュリティとベストプラクティス](#セキュリティとベストプラクティス)
10. [よく使う機能](#よく使う機能)

---

## GitHubとは

### GitHubの概要
GitHubは、Gitを使用したバージョン管理システムをベースにした、ソフトウェア開発のためのプラットフォームです。

**主な特徴：**
- **コードのホスティング**: リポジトリの保存と管理
- **コラボレーション**: チーム開発のサポート
- **Issue管理**: バグ報告や機能要求の管理
- **プルリクエスト**: コードレビューとマージ
- **GitHub Pages**: 静的サイトのホスティング
- **GitHub Actions**: CI/CDパイプライン

### GitHubの利点
- **無料プラン**: 個人・小規模プロジェクトに最適
- **オープンソース**: 多くのプロジェクトが公開されている
- **コミュニティ**: 開発者コミュニティが活発
- **統合ツール**: 様々な開発ツールとの連携

---

## アカウント作成と初期設定

### 1. アカウント作成
```bash
# 1. GitHub.comにアクセス
# 2. "Sign up"をクリック
# 3. ユーザー名、メール、パスワードを入力
# 4. メール認証を完了
```

### 2. プロフィール設定
```markdown
# プロフィールページの設定項目
- プロフィール画像
- 名前と自己紹介
- 所属組織
- 場所
- ウェブサイト
- ソーシャルメディア
```

### 3. SSHキーの設定
```bash
# SSHキーの生成
ssh-keygen -t ed25519 -C "your_email@example.com"

# SSHキーの確認
cat ~/.ssh/id_ed25519.pub

# GitHubにSSHキーを追加
# Settings > SSH and GPG keys > New SSH key
```

### 4. Personal Access Tokenの作成
```bash
# GitHubでPersonal Access Tokenを作成
# Settings > Developer settings > Personal access tokens > Tokens (classic)

# 必要な権限
- repo (リポジトリへのアクセス)
- workflow (GitHub Actions)
- admin:org (組織管理)
```

---

## リポジトリの作成と管理

### 1. リポジトリの作成
```bash
# GitHubでの作成手順
1. "New repository"をクリック
2. リポジトリ名を入力
3. 説明を追加（オプション）
4. Public/Privateを選択
5. README、.gitignore、ライセンスを選択
6. "Create repository"をクリック
```

### 2. リポジトリのクローン
```bash
# HTTPSでクローン
git clone https://github.com/username/repository.git

# SSHでクローン
git clone git@github.com:username/repository.git

# 特定のブランチをクローン
git clone -b branch-name https://github.com/username/repository.git
```

### 3. リポジトリの設定
```bash
# リポジトリの説明を更新
# Settings > General > Description

# トピックの追加
# リポジトリページで「Add topics」をクリック

# ウェブサイトの設定
# Settings > Pages
```

### 4. リポジトリの削除
```bash
# 注意: 削除は取り消しできません
# Settings > Danger Zone > Delete this repository
```

---

## 基本的な操作

### 1. ファイルの作成と編集
```markdown
# GitHubでのファイル作成
1. リポジトリページで「Add file」をクリック
2. 「Create new file」を選択
3. ファイル名を入力
4. 内容を記述
5. コミットメッセージを入力
6. 「Commit new file」をクリック
```

### 2. ファイルのアップロード
```markdown
# ファイルのアップロード手順
1. 「Add file」>「Upload files」をクリック
2. ファイルをドラッグ&ドロップ
3. コミットメッセージを入力
4. 「Commit changes」をクリック
```

### 3. ファイルの編集
```markdown
# ファイルの編集手順
1. ファイルをクリック
2. 鉛筆アイコン（Edit this file）をクリック
3. 内容を編集
4. コミットメッセージを入力
5. 「Commit changes」をクリック
```

### 4. ファイルの削除
```markdown
# ファイルの削除手順
1. ファイルをクリック
2. ゴミ箱アイコンをクリック
3. 削除理由を入力
4. 「Commit changes」をクリック
```

---

## ブランチとプルリクエスト

### 1. ブランチの作成
```bash
# ローカルでブランチを作成
git checkout -b feature/new-feature

# GitHubでブランチを作成
1. ブランチドロップダウンをクリック
2. 「New branch」をクリック
3. ブランチ名を入力
4. 「Create branch」をクリック
```

### 2. ブランチの管理
```bash
# ブランチの一覧表示
git branch -a

# ブランチの切り替え
git checkout branch-name

# ブランチの削除
git branch -d branch-name
```

### 3. プルリクエストの作成
```markdown
# プルリクエストの作成手順
1. ブランチページで「Compare & pull request」をクリック
2. タイトルと説明を入力
3. レビュアーを指定
4. ラベルを追加
5. 「Create pull request」をクリック
```

### 4. プルリクエストのレビュー
```markdown
# レビューの手順
1. プルリクエストを開く
2. 「Files changed」タブを確認
3. コメントを追加
4. 「Review changes」をクリック
5. 承認または変更要求
```

### 5. マージの実行
```markdown
# マージの手順
1. プルリクエストページで「Merge pull request」をクリック
2. マージメッセージを確認
3. 「Confirm merge」をクリック
4. ブランチを削除（オプション）
```

---

## Issuesとプロジェクト管理

### 1. Issueの作成
```markdown
# Issueの作成手順
1. 「Issues」タブをクリック
2. 「New issue」をクリック
3. タイトルを入力
4. 詳細な説明を記述
5. ラベルを追加
6. アサイニーを指定
7. 「Submit new issue」をクリック
```

### 2. Issueテンプレート
```markdown
# .github/ISSUE_TEMPLATE/bug_report.md
---
name: Bug report
about: Create a report to help us improve
title: ''
labels: bug
assignees: ''

---

**Describe the bug**
A clear and concise description of what the bug is.

**To Reproduce**
Steps to reproduce the behavior:
1. Go to '...'
2. Click on '....'
3. Scroll down to '....'
4. See error

**Expected behavior**
A clear and concise description of what you expected to happen.

**Screenshots**
If applicable, add screenshots to help explain your problem.

**Environment:**
 - OS: [e.g. macOS]
 - Browser: [e.g. chrome, safari]
 - Version: [e.g. 22]

**Additional context**
Add any other context about the problem here.
```

### 3. プロジェクトボード
```markdown
# プロジェクトボードの作成
1. 「Projects」タブをクリック
2. 「New project」をクリック
3. テンプレートを選択
4. プロジェクト名を入力
5. 「Create project」をクリック
```

### 4. マイルストーン
```markdown
# マイルストーンの作成
1. 「Issues」>「Milestones」をクリック
2. 「New milestone」をクリック
3. タイトルと説明を入力
4. 期限を設定
5. 「Create milestone」をクリック
```

---

## GitHub Pages

### 1. GitHub Pagesの設定
```bash
# GitHub Pagesの有効化
1. Settings > Pages
2. Sourceを選択（mainブランチなど）
3. フォルダを選択（/root または /docs）
4. 「Save」をクリック
```

### 2. カスタムドメインの設定
```bash
# カスタムドメインの追加
1. Settings > Pages
2. Custom domainにドメインを入力
3. 「Save」をクリック
4. DNSレコードを設定
```

### 3. Jekyllサイトの作成
```yaml
# _config.yml
title: My GitHub Pages Site
description: A sample site
author: Your Name
email: your-email@example.com

# テーマの設定
theme: jekyll-theme-cayman
```

---

## GitHub Actions

### 1. 基本的なワークフロー
```yaml
# .github/workflows/ci.yml
name: CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Setup Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18'
        
    - name: Install dependencies
      run: npm install
      
    - name: Run tests
      run: npm test
```

### 2. デプロイメントワークフロー
```yaml
# .github/workflows/deploy.yml
name: Deploy to Production

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Deploy to server
      run: |
        echo "Deploying to production..."
        # デプロイスクリプト
```

### 3. セキュリティスキャン
```yaml
# .github/workflows/security.yml
name: Security Scan

on:
  schedule:
    - cron: '0 0 * * 0'  # 毎週日曜日

jobs:
  security:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Run security scan
      uses: github/codeql-action/init@v2
      with:
        languages: javascript
```

---

## セキュリティとベストプラクティス

### 1. セキュリティ設定
```bash
# 2要素認証の有効化
1. Settings > Security > Two-factor authentication
2. 「Enable two-factor authentication」をクリック
3. 認証アプリまたはSMSを選択
4. 設定を完了
```

### 2. リポジトリのセキュリティ
```bash
# ブランチ保護ルール
1. Settings > Branches
2. 「Add rule」をクリック
3. ブランチ名パターンを入力
4. 保護オプションを設定
   - Require pull request reviews
   - Require status checks to pass
   - Require branches to be up to date
```

### 3. シークレットの管理
```bash
# リポジトリシークレットの追加
1. Settings > Secrets and variables > Actions
2. 「New repository secret」をクリック
3. 名前と値を入力
4. 「Add secret」をクリック
```

### 4. 依存関係の脆弱性スキャン
```bash
# Dependabotの有効化
1. Settings > Security & analysis
2. 「Enable」をクリック
3. 設定をカスタマイズ
```

---

## よく使う機能

### 1. 検索機能
```markdown
# GitHub検索の使い方
- リポジトリ検索: repo:username/repository
- 言語検索: language:javascript
- スター数検索: stars:>1000
- 更新日検索: pushed:>2023-01-01
- ファイル検索: filename:package.json
```

### 2. フォークとコントリビューション
```bash
# リポジトリのフォーク
1. リポジトリページで「Fork」をクリック
2. フォーク先を選択
3. フォークが完了

# プルリクエストの作成
1. フォークしたリポジトリで変更
2. プルリクエストを作成
3. 元のリポジトリにマージ
```

### 3. リリースの作成
```bash
# リリースの作成手順
1. 「Releases」をクリック
2. 「Create a new release」をクリック
3. タグを選択または作成
4. タイトルと説明を入力
5. バイナリファイルをアップロード
6. 「Publish release」をクリック
```

### 4. Wikiの作成
```markdown
# Wikiの作成
1. 「Wiki」タブをクリック
2. 「Create the first page」をクリック
3. ページ名と内容を入力
4. 「Save Page」をクリック
```

### 5. ディスカッション
```markdown
# ディスカッションの有効化
1. Settings > Features
2. 「Discussions」を有効化
3. カテゴリを設定
4. ディスカッションを開始
```

---

## トラブルシューティング

### 1. よくある問題

#### プッシュが拒否された
```bash
# 解決方法
git pull origin main
git push origin main

# または強制プッシュ（注意）
git push --force origin main
```

#### マージコンフリクト
```bash
# 解決手順
1. コンフリクトファイルを確認
2. コンフリクトマーカーを削除
3. 正しい内容に修正
4. ファイルをステージング
5. マージを完了
```

#### 認証エラー
```bash
# Personal Access Tokenの再設定
1. 古いトークンを削除
2. 新しいトークンを作成
3. 適切な権限を設定
4. 認証情報を更新
```

### 2. パフォーマンスの改善
```bash
# 大きなファイルの管理
1. Git LFSを使用
2. .gitignoreで除外
3. 履歴のクリーンアップ

# リポジトリの最適化
git gc
git prune
```

---

## 参考リンク
- [GitHub公式ドキュメント](https://docs.github.com/)
- [GitHub Guides](https://guides.github.com/)
- [GitHub Learning Lab](https://lab.github.com/)
- [GitHub Skills](https://skills.github.com/)

---

## まとめ

GitHubは、現代のソフトウェア開発において不可欠なプラットフォームです。このガイドで紹介した機能を活用することで、効率的で安全な開発プロセスを構築できます。

**重要なポイント：**
- セキュリティを最優先に考える
- コミュニケーションを大切にする
- 継続的な学習を心がける
- ベストプラクティスに従う

*このガイドは継続的に更新されます。最新の情報については公式ドキュメントを参照してください。* 