# Github Actions sandbox
Github Actionsのテスト用ディレクトリ

## Four Keys Metrics 計測のためのブランチ戦略

このリポジトリでは、Four Keys メトリクス（Deployment Frequency、Lead Time for Changes、Mean Time to Recovery、Change Failure Rate）を効果的に計測するため、以下のブランチ戦略を推奨します。

### ブランチ構成

```
main (production)
├── stg (staging)
└── feature/* (feature branches)
    ├── feature/short-description/develop (large feature integration)
    ├── feature/short-description/subtask-1
    ├── feature/short-description/subtask-2
    └── hotfix/* (hotfix branches)
```

### ブランチルール

#### main ブランチ
- **用途**: 本番環境デプロイ用
- **保護設定**: 直接プッシュ禁止、PR レビュー必須
- **マージ方式**: マージコミット（履歴保持のため）
- **マージ条件**: ステージング環境でのテスト完了後のみ
- **コミット規約**: 
  - `feat:` - 新機能
  - `fix:` - バグ修正
  - `hotfix:` - 緊急修正
  - `revert:` - リバート

#### stg ブランチ
- **用途**: ステージング環境デプロイ用
- **マージ元**: feature/* ブランチから PR
- **テスト**: 統合テスト実行後 main へマージ

#### feature/* ブランチ（小規模変更）
- **命名規則**: `feature/JIRA-123-description` または `feature/short-description`
- **作成元**: main ブランチから分岐
- **マージ先**: stg ブランチ（テスト後 main へ）

#### feature/*/develop ブランチ（大規模変更統合用）
- **命名規則**: `feature/large-feature/develop`
- **用途**: 大規模機能開発の統合ブランチ
- **作成元**: main ブランチから分岐
- **マージ元**: subtask ブランチから PR
- **マージ先**: stg ブランチ
- **レビュー**: 中間レビューで進捗確認・方向性調整

#### feature/*/subtask ブランチ（サブタスク）
- **命名規則**: `feature/large-feature/subtask-description`
- **用途**: 大規模機能の個別サブタスク開発
- **作成元**: feature/*/develop ブランチから分岐
- **マージ先**: feature/*/develop ブランチ
- **レビュー**: サブタスク完了時に中間レビュー実施

#### hotfix/* ブランチ
- **命名規則**: `hotfix/urgent-fix-description`
- **作成元**: main ブランチから直接分岐
- **マージ先**: main ブランチ（緊急時は直接）

### Four Keys 計測における意味

1. **Deployment Frequency**: 本番環境デプロイワークフロー（deploy.yml）の成功実行回数で計測
2. **Lead Time for Changes**: PR 作成から main マージまでの時間
3. **Mean Time to Recovery**: hotfix/ ブランチによる修正時間
4. **Change Failure Rate**: hotfix, revert, rollback コミットの割合

### ワークフロー

#### 小規模変更フロー
1. 開発者は feature/* ブランチで開発
2. stg ブランチへ PR を作成してテスト
3. ステージング環境でのテスト完了後、main ブランチへ PR を作成
4. レビュー・承認後、main へマージ（デプロイメント）

#### 大規模変更フロー
1. 機能責任者が feature/large-feature/develop ブランチを作成
2. 開発者は feature/large-feature/subtask-* ブランチで個別タスクを開発
3. サブタスク完了時、feature/large-feature/develop へ PR を作成・中間レビュー
4. 全サブタスク完了後、feature/large-feature/develop から stg ブランチへ PR
5. ステージング環境でのテスト完了後、main ブランチへ PR を作成
6. 最終レビュー・承認後、main へマージ（デプロイメント）

#### 緊急対応フロー
1. 問題発生時は hotfix/* ブランチで緊急対応
2. 修正後、直接 main ブランチへ PR を作成
3. 緊急レビュー・承認後、main へマージ

### 中間レビューのメリット
- 大規模変更の進捗確認と方向性調整
- 早期の設計問題発見とフィードバック
- チーム間の知識共有促進
- 最終レビュー時の負荷軽減

この戦略により、毎週土曜日の Four Keys メトリクス計測が正確に行われます。
