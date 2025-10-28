# Git コマンド練習用プロジェクト

このプロジェクトは、Git の重要なコマンドを理解し、実際に練習するためのリソース集です。

## 📁 プロジェクト構造

```
git_practice/
├── README.md                           # このファイル
├── SETUP_GUIDE.md                      # 詳細セットアップガイド
├── docs/                              # ドキュメント集
│   ├── 01-inspection-commands.md       # 確認系コマンド (log, status, diff, show等)
│   ├── 02-branch-operations.md         # ブランチ操作系コマンド
│   ├── 03-merge-vs-rebase.md          # Merge と Rebase の違い
│   ├── 04-reset-vs-revert.md          # Reset と Revert の違い
│   ├── 05-other-important-commands.md  # その他重要コマンド (cherry-pick, stash, reflog, tag等)
│   └── 06-submodule-operations.md      # Submodule操作とプロジェクト管理
├── practice/                          # 実習用ファイル（submoduleとして管理）
│   ├── sample-project/                # 練習用のサンプルプロジェクト
│   │   ├── main.py                    # Python サンプルファイル
│   │   ├── config.json                # 設定ファイルサンプル
│   │   └── README.md                  # プロジェクトの説明
│   └── PRACTICE_GUIDE.md              # 実践的な練習ガイド
└── .gitignore                         # Git除外設定
```

## 🎯 学習の進め方

1. **基礎確認**: `docs/01-inspection-commands.md` で状況確認系のコマンドを学習
2. **ブランチ操作**: `docs/02-branch-operations.md` でブランチの作成・切り替えを学習
3. **重要な違いの理解**: 
   - `docs/03-merge-vs-rebase.md` で merge と rebase の違いを理解
   - `docs/04-reset-vs-revert.md` で reset と revert の違いを理解
4. **応用コマンド**: `docs/05-other-important-commands.md` でその他の重要コマンドを学習
5. **Submodule管理**: `docs/06-submodule-operations.md` でSubmoduleの管理方法を学習
6. **実践練習**: `practice/PRACTICE_GUIDE.md` で実際にコマンドを試す

## 🚀 セットアップ

### 📖 詳細なセットアップガイド

すべてのセットアップ方法の詳細は **[SETUP_GUIDE.md](SETUP_GUIDE.md)** を参照してください。

### 基本セットアップ（通常利用）

実習用の Git リポジトリを作成するには：

```cmd
cd practice
# 練習環境のセットアップ
mkdir temp-repo
cd temp-repo
git init
copy ..\sample-project\* .
git add .
git commit -m "Initial commit: Add sample project files"
```

### Submodule としてのセットアップ（推奨）

このプロジェクトをSubmoduleとして管理する場合の詳細な手順は **[SETUP_GUIDE.md](SETUP_GUIDE.md#submodule構成セットアップ)** を参照してください。

#### クイックセットアップ
```cmd
# 1. 親リポジトリ初期化
git init
git add README.md docs/ .gitignore
git commit -m "Initial commit: Add documentation"

# 2. practice を独立リポジトリ化
cd practice
git init
git add .
git commit -m "Initial practice repository"
cd ..

# 3. Submodule として追加
git submodule add ./practice practice
git add .gitmodules practice
git commit -m "Add practice as submodule"

# 4. 初期化と更新
git submodule init
git submodule update
```

詳細な説明とトラブルシューティングは `docs/06-submodule-operations.md` を参照してください。

## 📝 注意事項

- このプロジェクト自体も Git で管理されているため、練習用の Git 操作は `practice/` ディレクトリ内で行ってください
- 各ドキュメントには Mermaid 記法による図解が含まれています
- 実際のコマンド例とその結果を示しているため、自分の環境で試してみることを推奨します

## 🔍 主要学習コマンド

| コマンド | 分類 | 説明 |
|---------|------|------|
| `git log` | 確認 | コミット履歴の表示 |
| `git status` | 確認 | 作業ツリーの状態確認 |
| `git diff` | 確認 | 変更差分の表示 |
| `git branch` | ブランチ | ブランチの作成・確認 |
| `git merge` | 統合 | ブランチのマージ |
| `git rebase` | 統合 | コミット履歴の再構築 |
| `git revert` | 取り消し | コミットの取り消し |
| `git reset` | 取り消し | コミットの巻き戻し |
| `git cherry-pick` | 応用 | 特定コミットの適用 |
| `git stash` | 応用 | 変更の一時保存 |
| `git submodule` | 管理 | Submoduleの管理 |

Happy Git learning! 🎉