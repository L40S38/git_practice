# 02. ブランチ操作系コマンド - ブランチの作成・切り替え・管理

Git におけるブランチ操作の基本的なコマンドについて説明します。ブランチは Git の最も強力な機能の一つです。

## 📋 目次

- [git branch - ブランチの管理](#git-branch---ブランチの管理)
- [git checkout - ブランチの切り替え（従来方式）](#git-checkout---ブランチの切り替え従来方式)
- [git switch - ブランチの切り替え（新方式）](#git-switch---ブランチの切り替え新方式)
- [git restore - ファイルの復元](#git-restore---ファイルの復元)
- [ブランチ戦略の基本](#ブランチ戦略の基本)

---

## git branch - ブランチの管理

### 📖 概要
ブランチの作成、一覧表示、削除、名前変更などを行います。

### 💡 基本的な使い方

```bash
# ブランチ一覧表示
git branch                 # ローカルブランチのみ
git branch -a             # すべてのブランチ（リモート含む）
git branch -r             # リモートブランチのみ

# ブランチ作成
git branch new-feature    # 現在のコミットから新しいブランチを作成
git branch new-feature commit-hash  # 特定のコミットから作成

# ブランチ削除
git branch -d feature-branch    # マージ済みブランチの削除
git branch -D feature-branch    # 強制削除（未マージでも削除）

# ブランチ名変更
git branch -m old-name new-name     # 他のブランチの名前変更
git branch -m new-name             # 現在のブランチの名前変更

# 詳細情報付きで表示
git branch -v             # 最新コミット情報も表示
git branch -vv            # 追跡ブランチ情報も表示

# マージ状況の確認
git branch --merged       # マージ済みブランチ
git branch --no-merged    # 未マージブランチ
```

### 📝 出力例

```
* main                    # 現在のブランチ（*付き）
  feature-login
  feature-dashboard
  remotes/origin/main
  remotes/origin/develop
```

---

## git checkout - ブランチの切り替え（従来方式）

### 📖 概要
ブランチの切り替えやファイルの復元を行う多機能コマンドです。Git 2.23 以降は `git switch` と `git restore` に分離されました。

### 💡 基本的な使い方

```bash
# ブランチ切り替え
git checkout branch-name

# ブランチ作成と同時に切り替え
git checkout -b new-feature

# 特定のコミットからブランチ作成
git checkout -b new-feature commit-hash

# 特定のコミットに移動（detached HEAD状態）
git checkout commit-hash

# ファイルの復元（作業ディレクトリの変更を破棄）
git checkout -- filename.txt
git checkout HEAD -- filename.txt

# 前のブランチに戻る
git checkout -

# リモートブランチからローカルブランチ作成
git checkout -b local-branch origin/remote-branch
```

### ⚠️ 注意事項

`git checkout` は多機能すぎるため、以下の問題があります：

1. **混乱しやすい**: ブランチ切り替えとファイル復元が同じコマンド
2. **危険性**: 意図しないファイルの上書きが発生する可能性
3. **可読性**: コマンドの意図が分かりにくい

そのため、Git 2.23 以降では `git switch` と `git restore` の使用が推奨されています。

---

## git switch - ブランチの切り替え（新方式）

### 📖 概要
ブランチの切り替えに特化したコマンドです。`git checkout` のブランチ切り替え機能を分離したものです。

### 💡 基本的な使い方

```bash
# ブランチ切り替え
git switch branch-name

# ブランチ作成と同時に切り替え
git switch -c new-feature
git switch --create new-feature

# 特定のコミットからブランチ作成
git switch -c new-feature commit-hash

# 前のブランチに戻る
git switch -

# リモートブランチからローカルブランチ作成
git switch -c local-branch origin/remote-branch

# detached HEAD状態でコミットに移動
git switch --detach commit-hash
```

### ✅ git switch の利点

1. **明確な意図**: ブランチ切り替えのみに特化
2. **安全性**: ファイルの誤った上書きを防ぐ
3. **直感的**: コマンド名から動作が推測しやすい

---

## git restore - ファイルの復元

### 📖 概要
ファイルの復元に特化したコマンドです。`git checkout` のファイル復元機能を分離したものです。

### 💡 基本的な使い方

```bash
# 作業ディレクトリのファイルを復元（変更を破棄）
git restore filename.txt

# 複数ファイルの復元
git restore file1.txt file2.txt

# すべてのファイルを復元
git restore .

# ステージングエリアからファイルを復元（unstage）
git restore --staged filename.txt

# 特定のコミットからファイルを復元
git restore --source=commit-hash filename.txt
git restore --source=HEAD~1 filename.txt

# ステージングエリアと作業ディレクトリの両方を復元
git restore --staged --worktree filename.txt
```

### 📊 restore の動作パターン

```mermaid
flowchart LR
    A[作業ディレクトリ] --> B[ステージングエリア]
    B --> C[最新コミット]
    
    subgraph "restore オプション"
        D["git restore<br/>(作業ディレクトリを復元)"]
        E["git restore --staged<br/>(ステージングを復元)"]
        F["git restore --staged --worktree<br/>(両方を復元)"]
    end
    
    D -.-> A
    E -.-> B
    F -.-> A
    F -.-> B
```

---

## ブランチ戦略の基本

### 🌳 基本的なブランチフロー

```mermaid
gitgraph
    commit id: "Initial"
    commit id: "Setup"
    
    branch feature-login
    checkout feature-login
    commit id: "Add login form"
    commit id: "Add validation"
    
    checkout main
    commit id: "Fix typo"
    
    checkout feature-login
    commit id: "Add tests"
    
    checkout main
    merge feature-login
    commit id: "Release v1.1"
    
    branch feature-dashboard
    checkout feature-dashboard
    commit id: "Add dashboard"
    commit id: "Add charts"
    
    checkout main
    merge feature-dashboard
    commit id: "Release v1.2"
```

### 🔄 一般的なブランチ操作の流れ

```bash
# 1. 新機能の開発開始
git switch main                    # mainブランチに移動
git pull origin main              # 最新の状態に更新
git switch -c feature-new-login   # 新しい機能ブランチ作成

# 2. 開発作業
# ファイルを編集...
git add .
git commit -m "Add new login feature"

# 3. 定期的にmainの変更を取り込み
git switch main
git pull origin main
git switch feature-new-login
git merge main                    # または git rebase main

# 4. 機能完成後
git switch main
git merge feature-new-login       # 機能ブランチをマージ
git branch -d feature-new-login   # 不要になったブランチを削除
git push origin main              # リモートに反映
```

### 📋 ブランチ管理のベストプラクティス

#### ✅ 良い習慣

```bash
# 分かりやすいブランチ名
git switch -c feature/user-authentication
git switch -c fix/login-bug
git switch -c hotfix/security-patch

# ブランチの状況確認
git branch -vv                    # 追跡ブランチの確認
git branch --merged              # マージ済みブランチの確認

# 不要なブランチの整理
git branch -d $(git branch --merged | grep -v main)  # マージ済みブランチを一括削除
```

#### ❌ 避けるべき習慣

```bash
# あいまいなブランチ名
git switch -c test
git switch -c temp
git switch -c fix

# 長期間残る機能ブランチ
# -> 定期的にmainの変更を取り込み、早期マージを心がける
```

### 🔄 ブランチ切り替え時の注意点

```mermaid
flowchart TD
    A[ブランチ切り替え前] --> B{未コミットの変更あり？}
    B -->|Yes| C[git status で確認]
    C --> D[git add & git commit または git stash]
    D --> E[git switch branch-name]
    B -->|No| E
    E --> F[切り替え完了]
    
    G[変更が残っている場合] --> H[git stash で一時保存]
    H --> I[ブランチ切り替え]
    I --> J[git stash pop で復元]
```

### 💡 よく使うブランチ操作のエイリアス設定

```bash
# .gitconfig に設定すると便利
git config --global alias.sw switch
git config --global alias.swc 'switch -c'
git config --global alias.br branch
git config --global alias.brd 'branch -d'

# 使用例
git sw main                # git switch main と同じ
git swc new-feature       # git switch -c new-feature と同じ
git br -v                 # git branch -v と同じ
```

## 🚨 トラブルシューティング

### ブランチ切り替えできない場合

```bash
# エラー: "Your local changes to the following files would be overwritten"
# 解決方法1: 変更をコミット
git add .
git commit -m "WIP: 一時保存"

# 解決方法2: 変更を一時保存
git stash
git switch other-branch
git stash pop  # 必要に応じて

# 解決方法3: 変更を破棄（注意！）
git restore .
```

### detached HEAD状態から復帰

```bash
# detached HEAD状態で作業してしまった場合
git switch -c rescue-branch  # 現在の状態でブランチ作成
git switch main             # mainに戻る
git merge rescue-branch     # 必要に応じてマージ
```

## 📚 次のステップ

ブランチ操作に慣れたら、次は [03. Merge と Revert の違い](03-merge-vs-revert.md) に進んで、ブランチの統合と変更の取り消しについて学びましょう。