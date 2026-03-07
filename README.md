# my-notes (Quartz Knowledge Base Template)

Markdownで知識を整理し、Quartz + GitHub Pages で公開するための初期テンプレートです。  
執筆は Obsidian を前提にしています。

## 目的

- 1記事1テーマで知識を蓄積する
- GitHub Actions で自動ビルド・自動公開する
- 短文は note、詳細は Quartz 側に集約する

## ディレクトリ構成

```text
my-notes/
├─ content/
│  ├─ index.md
│  ├─ tech/
│  │  └─ index.md
│  ├─ perfume/
│  │  ├─ index.md
│  │  ├─ materials/
│  │  │  └─ index.md
│  │  ├─ formulas/
│  │  │  └─ index.md
│  │  └─ logs/
│  │     └─ index.md
│  ├─ coffee/
│  │  └─ index.md
│  ├─ logs/
│  │  └─ index.md
│  ├─ assets/
│  │  ├─ perfume/
│  │  ├─ tech/
│  │  └─ coffee/
│  └─ drafts/
├─ templates/
│  ├─ article.md
│  ├─ log.md
│  └─ list.md
├─ .github/workflows/
│  └─ deploy.yml
├─ quartz.config.ts
├─ quartz.layout.ts
├─ package.json
└─ README.md
```

## 執筆ルール (Markdown / Obsidian)

- 1記事 = 1テーマ
- ファイル名: 英小文字 + ハイフン区切り（例: `ambroxan.md`, `playwright-coverage.md`）
- 見出し構成:
  - `# Title`
  - `## Summary`
  - `## Details`
  - `## Notes`
  - `## Related`
- 見出しは4階層以上にしない
- 内部リンクは Obsidian の wikilink を使用（例: `[[Ambroxan]]`）
- 画像は `content/assets/` 配下に保存し、本文では `/assets/...` で参照

### タグ設計

タグは3階層以内に統一します。

- `perfume/material`
- `perfume/musk`
- `tech/testing`
- `tech/flask`
- `coffee/beans`
- `coffee/brewing`
- `type/reference`
- `type/log`
- `status/draft`
- `status/evergreen`

## Obsidian運用ルール

### 1. Vault設定

- このリポジトリ直下を Obsidian Vault として開く
- `Settings > Files and links`
  - Default location for new notes: `content`
  - New link format: `Shortest path when possible`
  - Use `[[Wikilink]]`

### 2. Templates設定

- `Settings > Core plugins` で **Templates** を有効化
- `Template folder location` を `templates` に設定
- 記事種別に応じて以下を使用
  - 通常記事: `templates/article.md`
  - 日次ログ: `templates/log.md`
  - 一覧ページ: `templates/list.md`

### 3. 新規記事作成手順

1. 対象カテゴリへ移動（例: `content/tech/`）
2. 新規ファイル作成（例: `playwright-coverage.md`）
3. `article.md` テンプレートを挿入
4. frontmatter の `title`, `tags`, `status`, `created`, `updated` を記入
5. `Related` で関連ノートにリンク

## 更新フロー

```text
Obsidian
↓
Markdownを書く
↓
git commit
↓
git push
↓
GitHub Actions
↓
Quartz build
↓
GitHub Pages公開
```

> 生成物（`public/` など）はGitにコミットしません。

## Git運用

- デフォルトブランチ: `main`
- 推奨コミット単位: 「1テーマの追記」「1カテゴリの整理」
- コミットメッセージ例
  - `docs(tech): add flask-authentication note`
  - `docs(perfume): add ambroxan material memo`

## note連携方法

- 短文・速報: noteへ投稿
- 詳細版: Quartz側のノートで管理
- note本文末尾に「詳細はこちら」として Quartz記事URLを記載
- Quartz記事側 `## Notes` へ note記事URLを追記して相互リンク

## ローカル起動方法

```bash
npm ci
npx quartz build --serve
```

プレビュー: `http://localhost:8080`

## GitHub Pages公開設定

1. GitHub Repository の **Settings > Pages** を開く
2. Source を **GitHub Actions** に設定
3. `quartz.config.ts` の `baseUrl` を `your-username.github.io/my-notes` 形式に変更
4. `main` に push すると `.github/workflows/deploy.yml` で自動公開
