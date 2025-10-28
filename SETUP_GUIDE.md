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
- `app.py` - Pythonサンプルアプリケーション
- `config.json` - 設定ファイル
- `requirements.txt` - 依存関係

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

`PRACTICE_GUIDE.md` の手順に従ってGitコマンドの練習を開始できます。

---

## Submodule構成セットアップ

### 🎯 目的
プロジェクト全体をSubmodule構成に変更し、ドキュメント（親リポジトリ）と練習環境（Submodule）を分離して管理します。

### 🚀 完全セットアップガイド

#### Phase 1: 事前確認と準備

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

#### Phase 2: 親リポジトリの初期化

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

#### Phase 3: practice ディレクトリの直接Submodule化

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

#### Phase 4: Submodule設定

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

#### Phase 5: Submoduleの初期化と確認

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

### 🌐 リモートリポジトリ版セットアップ

より本格的な環境で運用する場合：

**1. practice ディレクトリをリモートリポジトリにプッシュ**
```cmd
# Phase 4のStep 4完了後、practice ディレクトリに移動
cd practice

# リモートリポジトリを追加
git remote add origin <your-practice-repository-url>

# リモートにプッシュ
git push -u origin main

# 親ディレクトリに戻る
cd ..
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

#### 4. コミットエラー
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