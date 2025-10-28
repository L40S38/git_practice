# セットアップガイド - Git コマンド練習プロジェクト

このドキュメントでは、Git コマンド練習プロジェクトの各種セットアップ方法を詳細に説明します。

## 📋 目次

- [基本セットアップ（練習環境）](#基本セットアップ練習環境)
- [Submodule構成セットアップ](#submodule構成セットアップ)
- [トラブルシューティング](#トラブルシューティング)

---

## 基本セットアップ（練習環境）

### 🎯 目的
`practice/` ディレクトリ内で実際にGitコマンドを練習するための環境を構築します。

### 📝 手動セットアップ手順

#### Step 1: practice ディレクトリに移動
```cmd
cd practice
```

#### Step 2: 練習用ディレクトリの準備
```cmd
# 既存のtemp-repoがある場合は削除
if exist temp-repo rmdir /s /q temp-repo

# 新しい練習用ディレクトリを作成
mkdir temp-repo
cd temp-repo
```

#### Step 3: Gitリポジトリの初期化
```cmd
git init
```

#### Step 4: サンプルファイルのコピー
```cmd
# sample-projectからファイルをコピー
copy ..\sample-project\* .

# コピーされたファイルを確認
dir
```

サンプルファイルには以下が含まれます：
- `main.py` - Pythonサンプルスクリプト
- `config.json` - 設定ファイル
- `README.md` - 手順のサマリー

#### Step 5: 初期コミットの作成
```cmd
git add .
git commit -m "Initial commit: Add sample project files"
```

#### Step 6: セットアップ確認
```cmd
# 現在のブランチ情報
git branch

# リポジトリの状態確認
git status

# コミット履歴の確認
git log --oneline

# 作成されたファイルの確認
dir
```

### ✅ セットアップ完了

これで以下の状態になります：
- Gitリポジトリが初期化されている
- サンプルファイルが追加されている
- 初期コミットが作成されている
- `main` ブランチで作業可能

#### 🌐 練習環境のリモート管理（任意）

練習環境をリモートリポジトリで管理したい場合：

**Step 1: 現在のブランチ名を確認**
```cmd
# 現在のブランチ名を確認
git branch

# 詳細な状態確認
git status
```

**新規リモートリポジトリの場合**
```cmd
# リモートリポジトリを追加
git remote add origin <your-practice-repo-url>

# 現在のブランチ名に応じて初回プッシュ
# ブランチが "main" の場合
git push -u origin main

# ブランチが "master" の場合
git push -u origin master

# 以降の作業後は簡単にプッシュ
git add .
git commit -m "Practice: <何を練習したか>"
git push
```

**既存リモートがある場合**
```cmd
# リモート設定確認
git remote -v

# 必要に応じてURL変更
git remote set-url origin <new-repo-url>

# 現在のブランチ名に応じてプッシュ
git push origin main    # mainブランチの場合
git push origin master  # masterブランチの場合
```

**⚠️ Push時のトラブル解決**
```cmd
# "No configured push destination" エラーの場合
git remote add origin <repository-url>
git push -u origin main

# "no upstream branch" エラーの場合
git push -u origin main

# "Updates were rejected" エラーの場合
git pull origin main  # または git pull --rebase origin main
git push origin main
```

`PRACTICE_GUIDE.md` の手順に従ってGitコマンドの練習を開始できます。

---

## Submodule構成セットアップ

### 🎯 目的
プロジェクト全体をSubmodule構成に変更し、ドキュメント（親リポジトリ）と練習環境（Submodule）を分離して管理します。

### 🚀 完全セットアップガイド

#### Step 1: 事前確認と準備

**1. ディレクトリ構成の確認**
```cmd
# プロジェクトルートにいることを確認
dir

# 必要なディレクトリが存在することを確認
# ✓ docs (ドキュメントディレクトリ)
# ✓ practice (練習用ディレクトリ)
```

**2. 既存の.gitディレクトリの確認**
```cmd
# 既存のGitリポジトリがあるかチェック
if exist .git (
    echo 既存のGitリポジトリが検出されました
) else (
    echo 新規Gitリポジトリを作成します
)
```

#### Step 2: 親リポジトリの初期化

**3. 親リポジトリの設定**
```cmd
# Gitリポジトリを初期化（まだの場合）
git init

# グローバル設定の確認（必要に応じて設定）
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# ドキュメントファイルをステージング
git add README.md docs/ .gitignore

# 初期コミット作成
git commit -m "Initial commit: Add Git learning documentation"
```

#### Step 3: practice ディレクトリの独立リポジトリ化

**4. practice ディレクトリの独立リポジトリ化**
```cmd
# practice ディレクトリに移動
cd practice

# 独立したGitリポジトリとして初期化
git init

# 全ファイルをステージング
git add .

# 初期コミット作成
git commit -m "Initial practice repository with exercises and guides"

# practice ディレクトリが独立したリポジトリになったことを確認
git status

# 親ディレクトリに戻る
cd ..
```

#### Step 4: Submodule設定

**5. practice ディレクトリをSubmoduleとして追加**
```cmd
# 現在の practice ディレクトリを直接Submoduleとして追加
git submodule add ./practice practice

# .gitmodulesファイルが作成されたことを確認
type .gitmodules
```

**6. Submodule追加のコミット**
```cmd
# Submodule設定をステージング
git add .gitmodules practice

# Submodule追加をコミット
git commit -m "Add practice directory as submodule"
```

#### Step 5: Submoduleの初期化と確認

**7. Submoduleの初期化**
```cmd
# Submoduleを初期化
git submodule init

# Submoduleを更新
git submodule update

# Submoduleの状態確認
git submodule status
```

**8. 最終確認**
```cmd
# 全体の状態確認
git status

# practice ディレクトリ内のファイル確認
dir practice
```

#### Step 6: 親リポジトリのリモートプッシュ（任意）

プロジェクト全体をリモートリポジトリで管理する場合：

**Step 1: 現在のブランチ名を確認**
```cmd
# 現在のブランチ名を確認
git branch

# または詳細情報を確認
git status
```

**新規リモートリポジトリの場合**
```cmd
# リモートリポジトリを追加
git remote add origin <main-repository-url>

# 現在のブランチ名に応じてプッシュ
# ブランチが "main" の場合
git push -u origin main

# ブランチが "master" の場合
git push -u origin master

# 以降は簡単にプッシュ可能
git push
```

**ブランチ名統一（main に変更する場合）**
```cmd
# 現在のブランチ名を確認
git branch

# master から main にブランチ名を変更
git branch -m master main

# リモートにmainブランチとしてプッシュ
git push -u origin main

# リモートのmasterブランチを削除（必要に応じて）
git push origin --delete master
```

**既存リモートリポジトリがある場合**
```cmd
# 現在のリモート設定確認
git remote -v

# 必要に応じてリモートURL変更
git remote set-url origin <new-repository-url>

# プッシュ実行
git push origin main
```

### 🌐 リモートリポジトリ版セットアップ

より本格的な環境で運用する場合：

#### ケース1: 新規リモートリポジトリを作成する場合

**1. practice ディレクトリ（Submodule）をリモートリポジトリにプッシュ**
```cmd
# Step 3完了後、practice ディレクトリに移動
cd practice

# 現在のブランチとリモートの状態確認
git branch
git remote -v

# リモートリポジトリを追加（まだない場合）
git remote add origin <your-practice-repository-url>

# 現在のブランチに応じて初回プッシュ（upstream設定付き）
git push -u origin main    # mainブランチの場合
git push -u origin master  # masterブランチの場合

# 親ディレクトリに戻る
cd ..
```

**2. 親リポジトリもリモートにプッシュ**
```cmd
# 親リポジトリのリモート設定（まだの場合）
git remote add origin <your-main-repository-url>

# 親リポジトリをプッシュ（Submodule参照情報も含む）
git push -u origin main    # mainブランチの場合
git push -u origin master  # masterブランチの場合
```

**3. .gitmodules ファイルのURL更新（重要）**
```cmd
# 現在の.gitmodulesの内容を確認
type .gitmodules

# 出力例:
# [submodule "practice"]
#     path = practice
#     url = ./practice

# .gitmodulesのURLをリモートリポジトリのURLに変更
git config -f .gitmodules submodule.practice.url <your-practice-repository-url>

# 変更後の内容を確認
type .gitmodules

# 出力例:
# [submodule "practice"]
#     path = practice
#     url = https://github.com/username/practice.git

# .gitmodulesの変更をコミット
git add .gitmodules
git commit -m "Update submodule URL to remote repository"
git push origin main
```

**❗ なぜURL更新が必要か？**
- 相対パス `./practice` は他の人がクローンした時に動作しない
- リモートリポジトリのURLにすることで、どこからでもSubmoduleを取得可能
- チーム開発で必須の設定

#### ケース2: 既存のリモートリポジトリがある場合

**1. practice ディレクトリ（Submodule）の既存リモート確認と更新**
```cmd
# practice ディレクトリに移動
cd practice

# 現在のリモート設定とブランチを確認
git remote -v
git branch

# 既存のリモートがある場合はそのまま使用
# または新しいリモートに変更する場合
git remote set-url origin <new-repository-url>

# リモートにプッシュ（ブランチ名に応じて）
git push origin main    # mainブランチの場合
git push origin master  # masterブランチの場合

# 親ディレクトリに戻る
cd ..
```

**2. 親リポジトリの既存リモート確認と更新**
```cmd
# 親リポジトリのリモート設定確認
git remote -v
git branch

# 必要に応じてリモートURL変更
git remote set-url origin <new-main-repository-url>

# 親リポジトリをプッシュ（Submodule参照情報も含む）
git push origin main    # mainブランチの場合
git push origin master  # masterブランチの場合
```

**3. .gitmodules ファイルのURL更新（ケース2でも必要）**
```cmd
# 現在の.gitmodulesのURL確認
git config -f .gitmodules submodule.practice.url

# 相対パス（./practice）の場合は更新が必要
git config -f .gitmodules submodule.practice.url <actual-practice-repository-url>

# 変更をコミット
git add .gitmodules
git commit -m "Update submodule URL to remote repository"
git push origin main
```

#### 📋 Submodule更新後の推奨ワークフロー

**Submodule内で変更を行った場合：**
```cmd
# 1. Submodule内で作業とコミット
cd practice
# ファイル編集...
git add .
git commit -m "Update practice exercises"
git push origin main  # Submoduleのリモートにプッシュ
cd ..

# 2. 親リポジトリでSubmodule参照を更新
git add practice
git commit -m "Update practice submodule reference"
git push origin main  # 親リポジトリのリモートにプッシュ
```

#### 🚨 Push時のよくあるエラーと解決法

**Error 1: "error: src refspec main does not match any"**
```cmd
# 原因: ローカルブランチ名とプッシュ先が不一致
# 解決: 現在のブランチ名を確認してプッシュ
git branch  # ブランチ名確認
git push -u origin master  # masterブランチの場合
git push -u origin main    # mainブランチの場合
```

**Error 2: "fatal: No configured push destination"**
```cmd
# 原因: リモートリポジトリが設定されていない
# 解決: リモートを追加
git remote add origin <repository-url>
# 現在のブランチ名に応じてプッシュ
git push -u origin main    # mainブランチの場合
git push -u origin master  # masterブランチの場合
```

**Error 3: "fatal: The current branch main has no upstream branch"**
```cmd
# 原因: upstream（追跡ブランチ）が設定されていない
# 解決: -u オプションでupstreamを設定
git push -u origin main    # mainブランチの場合
git push -u origin master  # masterブランチの場合

# 以降は単純にpushできる
git push
```

**Error 4: "Updates were rejected because the remote contains work"**
```cmd
# 原因: リモートに新しいコミットがある
# 解決: まずfetchして確認
git fetch origin
git log --oneline --graph origin/main

# 必要に応じてmergeまたはrebase
git pull origin main
# または
git pull --rebase origin main

# その後pushを実行
git push origin main
```

**3. リモートからSubmoduleとして再追加する場合**
```cmd
# ローカルのpracticeディレクトリを削除
rmdir /s /q practice

# リモートリポジトリからSubmoduleとして追加
git submodule add <your-practice-repository-url> practice

# コミット
git add .gitmodules practice
git commit -m "Add practice directory as submodule from remote repository"
```

### 📝 Submodule日常管理のワークフロー

#### 🔄 通常の開発サイクル

**1. Submodule内での作業**
```cmd
# Submoduleディレクトリに移動
cd practice

# 最新の状態に更新
git pull origin main  # またはmaster

# 作業用ブランチを作成（推奨）
git checkout -b feature-new-exercise

# ファイル編集・追加
# （練習用コンテンツの追加、修正など）

# 変更をコミット
git add .
git commit -m "Add new Git branching exercise"

# リモートにプッシュ
git push origin feature-new-exercise

# メインブランチにマージ（またはPull Request作成）
git checkout main
git merge feature-new-exercise
git push origin main

# 作業ブランチを削除
git branch -d feature-new-exercise
git push origin --delete feature-new-exercise

# 親ディレクトリに戻る
cd ..
```

**2. 親リポジトリでの Submodule 参照更新**
```cmd
# Submoduleの新しいコミットを親リポジトリに反映
git add practice
git commit -m "Update practice submodule: Add new branching exercise"

# 親リポジトリをリモートにプッシュ
git push origin main
```

#### 👥 チーム開発での Submodule 管理

**他の開発者が Submodule を更新した場合：**
```cmd
# 親リポジトリの最新版を取得
git pull origin main

# Submodule参照が更新されていることを確認
git status

# Submoduleを最新のコミットに更新
git submodule update

# または強制更新
git submodule update --remote
```

**新しい環境でSubmodule込みプロジェクトを開始：**
```cmd
# リポジトリをクローン（Submodule込み）
git clone --recurse-submodules <main-repository-url>

# または既存クローンでSubmoduleを初期化
git clone <main-repository-url>
cd <repository-name>
git submodule init
git submodule update
```

**2. ローカルpracticeディレクトリを削除してリモートからSubmodule追加**
```cmd
# ローカルのpracticeディレクトリを削除
rmdir /s /q practice

# リモートリポジトリからSubmoduleとして追加
git submodule add <your-practice-repository-url> practice

# コミット
git add .gitmodules practice
git commit -m "Add practice directory as submodule from remote repository"
```

### 👥 他のユーザー向けセットアップ

Submodule構成のリポジトリを利用する場合：

**新規クローン（Submodule込み）**
```cmd
git clone --recurse-submodules <repository-url>
cd <repository-name>
```

**既存クローンでSubmoduleを取得**
```cmd
git clone <repository-url>
cd <repository-name>
git submodule init
git submodule update
```

---

## トラブルシューティング

### 🚨 よくある問題と解決法

#### 1. "git: command not found" エラー
```cmd
# Gitがインストールされているか確認
git --version

# インストールされていない場合は公式サイトからダウンロード
# https://git-scm.com/download/win
```

#### 2. ファイルコピーが失敗する
```cmd
# sample-projectディレクトリが存在するか確認
dir sample-project

# 権限の問題の場合、管理者権限でコマンドプロンプトを起動
```

#### 3. Submoduleが空ディレクトリのまま
```cmd
# Submoduleの初期化と更新を実行
git submodule init
git submodule update

# 強制的に更新
git submodule update --init --force
```

#### 4. Submodule URL関連エラー

**"fatal: repository './practice' does not exist" エラー（他の人がクローンした場合）**
```cmd
# 原因: .gitmodulesのURLが相対パスのまま
# 解決: .gitmodulesのURLを実際のリモートリポジトリURLに変更

# 現在のSubmodule URL確認
git config -f .gitmodules submodule.practice.url

# 相対パス（例: ./practice）の場合は変更が必要
git config -f .gitmodules submodule.practice.url https://github.com/username/practice.git

# 変更をコミット
git add .gitmodules
git commit -m "Fix submodule URL to use remote repository"
git push origin main
```

**Submoduleが古いURLを参照している場合**
```cmd
# 現在設定されているURL確認
git config -f .gitmodules --list

# 新しいURLに変更
git config -f .gitmodules submodule.practice.url <new-repository-url>

# Submoduleのローカル設定も更新
git submodule sync

# Submoduleを再初期化
git submodule update --init
```

#### 5. Git Push関連エラー

**"error: src refspec main does not match any" エラー**
```cmd
# 原因: ローカルブランチ名とプッシュ先ブランチ名が異なる
# 現在のブランチ名を確認
git branch

# masterブランチの場合
git push -u origin master

# ブランチ名をmainに変更したい場合
git branch -m master main
git push -u origin main
```

**"fatal: No configured push destination" エラー**
```cmd
# 原因: リモートリポジトリが設定されていない
# 解決法:
git remote add origin <repository-url>
# 現在のブランチ名に応じてプッシュ
git push -u origin main     # mainブランチの場合
git push -u origin master   # masterブランチの場合
```

**"fatal: The current branch has no upstream branch" エラー**
```cmd
# 原因: upstream（追跡ブランチ）が未設定
# 解決法:
git push -u origin main     # mainブランチの場合
git push -u origin master   # masterブランチの場合

# 以降は git push のみでOK
```

**"Updates were rejected because the remote contains work" エラー**
```cmd
# 原因: リモートに新しいコミットが存在
# 解決法1: マージしてからプッシュ
git pull origin main    # または git pull origin master
git push origin main    # または git push origin master

# 解決法2: リベースしてからプッシュ
git pull --rebase origin main    # または master
git push origin main             # または master
```

**"Permission denied (publickey)" エラー**
```cmd
# 原因: SSH認証の問題
# 解決法1: HTTPSのURLを使用
git remote set-url origin https://github.com/username/repo.git

# 解決法2: SSH鍵を設定（GitHub等）
# GitHubの設定でSSH鍵を追加
```

#### 5. コミットエラー
```cmd
# ユーザー設定が未設定の場合
git config user.name "Your Name"
git config user.email "your.email@example.com"

# またはグローバルに設定
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

### 💡 セットアップのベストプラクティス

#### ✅ 推奨事項
1. **事前バックアップ**: 重要なデータは事前にバックアップ
2. **段階的実行**: 各ステップを確認しながら進める
3. **状態確認**: `git status` で常に現在の状態を確認
4. **クリーンな環境**: 不要なファイルは事前に削除

#### ❌ 避けるべき事項
1. **途中でのディレクトリ移動**: セットアップ中は指定されたディレクトリで作業
2. **強制的な操作**: エラーが出た場合は原因を確認してから対処
3. **設定の省略**: Git ユーザー設定は必ず行う

### 🔧 環境の初期化

すべてをやり直したい場合：

```cmd
# Gitリポジトリの完全削除
rmdir /s /q .git

# temp-repoの削除
rmdir /s /q practice\temp-repo

# 最初からやり直し
# （このガイドの手順に従って再セットアップ）
```

---

## 📚 関連ドキュメント

- **[README.md](../README.md)** - プロジェクト概要とクイックスタート
- **[PRACTICE_GUIDE.md](../practice/PRACTICE_GUIDE.md)** - 具体的な練習手順
- **[06-submodule-operations.md](06-submodule-operations.md)** - Submoduleの詳細操作

## 🆘 サポート

セットアップでお困りの場合は、以下を確認してください：

1. **環境**: Windows コマンドプロンプトまたはPowerShell
2. **Git**: 最新版がインストール済み
3. **権限**: 必要に応じて管理者権限で実行
4. **パス**: プロジェクトルートディレクトリで実行