# Section 0: 前提知識 — Implementation Spec

## Goal
index.html の「0. 前提知識」ページ（id="page-prereq"）のプレースホルダーを削除し、
本コンテンツを実装する。
同時に、本章で確立する 3 つの共通 UI パターン（表 / 用語ツールチップ / コピーボタン）を
他章でも使えるよう汎用的に実装する。

## Target File
index.html (単一ファイル維持)

## Scope of This Change
- 0. 前提知識ページのコンテンツ実装
- 表の共通スタイル追加（他章でも使う）
- 用語ツールチップの共通仕組み追加
- セクションヘッダー横の Markdown コピーボタン共通仕組み追加
- 他のページ（page-prereq 以外）には変更を加えない

## Common UI Patterns to Establish

### 1. Table Style
- 横幅: 100%
- ヘッダー行: 背景 var(--bg-elevated)、テキスト var(--text-primary)、下に細いボーダー
- データ行: セル下に var(--border-subtle) の細い線
- パディング: 上下 10px、左右 14px
- フォントサイズ: 0.9rem
- 横スクロール対応（モバイル時）: ラッパー <div class="table-wrap"> で overflow-x: auto

HTML 構造:
```html
<div class="table-wrap">
  <table class="data-table">
    <thead><tr><th>...</th></tr></thead>
    <tbody><tr><td>...</td></tr></tbody>
  </table>
</div>
```

### 2. Term Tooltip
初出の専門用語に `*` を付け、ホバー（PC）/ タップ（モバイル）でツールチップ表示。

HTML 構造:
```html
<span class="term" data-tooltip="Business Unit. Marketing Cloud 内の組織分割単位...">
  BU<sup>*</sup>
</span>
```

CSS 要件:
- term: 下線（dotted, 1px, --text-tertiary）でツールチップ可能を示唆
- term:hover でツールチップ表示（::after 擬似要素を使う）
- ツールチップ本体:
  - 背景 var(--bg-elevated)
  - ボーダー 1px solid var(--border-default)
  - パディング 10px 14px
  - フォントサイズ 0.8rem
  - 最大幅 280px
  - 影なし or 控えめ
  - position: absolute, 上に表示（top: -8px, left: 0, transform: translateY(-100%)）
  - opacity 0 / visibility hidden をベースに、hover 時 opacity 1 / visibility visible
  - transition 0.18s

JS 要件:
- タップでもツールチップが出るようにする
- ドキュメントの他の場所をタップ/クリックすると閉じる
- 1 つの term をタップしたら他の開いている tooltip は閉じる

### 3. Section Copy Button
各セクション見出し（h2）の右側に控えめなコピーボタンを配置。
クリックでそのセクション全体の内容を Markdown 化してクリップボードにコピー。

HTML 構造:
```html
<section class="block" data-copyable>
  <div class="section-head">
    <h2 class="section-title">セクション見出し</h2>
    <button class="copy-btn" aria-label="このセクションをコピー" title="Markdown でコピー">
      <span class="copy-btn__icon"></span>
      <span class="copy-btn__label">Copy</span>
    </button>
  </div>
  <div class="section-body">
    <!-- 表 / リスト / 段落など -->
  </div>
</section>
```

CSS 要件:
- .section-head: flex で h2 とボタンを横並び、ボタン右寄せ、align-items center
- .copy-btn:
  - 背景透明
  - ボーダー 1px solid var(--border-subtle)
  - パディング 4px 10px
  - フォントサイズ 0.75rem
  - 色 var(--text-tertiary)
  - border-radius var(--radius)
  - hover で border-color var(--accent) / color var(--accent)
  - transition 0.18s
- .copy-btn__icon: 12px x 12px、シンプルなコピーアイコンを CSS のみで描画（疑似要素で枠とオフセット枠を表現）

JS 要件:
- 各 .copy-btn クリックで、対応する .section-body の内容を Markdown に変換
- 変換ルール:
  - h2 → `## `
  - h3 → `### `
  - p → そのままテキスト
  - ul/ol → Markdown のリスト記法
  - table → Markdown 表記法
  - <strong> / <em> → `**...**` / `*...*`
  - <code> → \`...\`
  - その他のタグはテキストのみ抽出
- navigator.clipboard.writeText() で書き込み
- コピー成功時、ボタンラベルを一時的に「Copied」に変更（1.5 秒）

## Section 0 Content to Implement

ページ構成（page-prereq の中身）:

### Page Header
```
<header class="page__header">
  <p class="page__subtitle">SECTION 0</p>
  <h1 class="page__title">前提知識：MC 全体構造の再確認</h1>
</header>
```

### Lead Paragraph
> Foundations（MC-101）で扱われるが、Email Specialist でも頻繁に問われる、
> もしくは頭に入っていることを前提として出題される領域。
> ここが曖昧だと他セクションの問題が解けない。

### Section 0.1: Marketing Cloud Engagement のアプリ構造

表（data-table）:

| アプリ | 役割 | Email Specialist での重要度 |
|---|---|---|
| Email Studio | メール送信、購読者管理、Tracking | ★★★ 中核 |
| Content Builder | コンテンツ作成・管理 | ★★★ 中核 |
| Automation Studio | バッチ自動化 | ★★★ 中核 |
| Journey Builder | 顧客行動ベースのフロー | ★★★ 中核 |
| Contact Builder | Contact Model、Population 管理 | ★★ 頻出 |
| Mobile Studio (MobileConnect/Push) | SMS/Push | ★ 名前と概念のみ |
| Analytics Builder | レポート | ★★ |
| Setup | BU/User/データ管理 | ★ 概念のみ |

### Section 0.2: Contact vs Subscriber の決定的な違い

リード文:
> ここを混同していると、購読管理・購読数差異・データ構造の問題が全滅する。

表:

| 概念 | 定義 | 識別子 | 存在場所 |
|---|---|---|---|
| Contact | チャネル横断の人物レコード | Contact Key | All Contacts (Contact Builder) |
| Subscriber | 特定チャネルの購読者レコード | Subscriber Key | All Subscribers (Email Studio) |
| Contact ID | システム内部の数値 ID（ユーザー変更不可） | Contact ID | バックエンド |

**重要な含意（h3 + ul）:**
- すべての Subscriber は Contact だが、すべての Contact が Subscriber ではない（メールを送られたことのない Contact もありうる）
- Sendable DE から行を削除しても、All Contacts からは消えない（Contact Identity は別レイヤーで管理）
- ベストプラクティス: Subscriber Key と Contact Key は同じ値にする
- Email Address を Contact Key にすると、メアド変更で別人扱い・共有メアドで競合などの問題が発生

### Section 0.3: Business Unit 構造（Enterprise 2.0）

リード文:
> 試験シナリオの多くが複数 BU を持つ企業を想定している。

ASCII ツリー図（pre タグで表示、フォント monospace、背景 var(--bg-surface)）:
```
Parent BU (Top-level Account)
├─ Child BU 1 (例: ブランドA)
├─ Child BU 2 (例: ブランドB)
└─ Child BU 3 (例: 部門X)
```

pre のスタイル:
- 背景 var(--bg-surface)
- ボーダー 1px solid var(--border-subtle)
- border-radius var(--radius)
- padding 16px 20px
- font-family ui-monospace, "SF Mono", Menlo, Consolas, monospace
- font-size 0.85rem
- color var(--text-secondary)
- overflow-x auto

ポイントリスト（h3「ポイント」+ ul）:
- All Contacts は Enterprise（全 BU 横断）レベル → Contact 数のカウントも Enterprise 単位
- All Subscribers は BU ごと（ただし Parent BU からは子 BU を跨いで参照可能）
- Shared Data Extension は Parent BU で作成し、複数の Child BU から参照可能
- Send Classification / Sender Profile / Delivery Profile は BU ごとに管理

### Section 0.4: 試験で出る前提

リスト（ul）:
- 多くのシナリオが「複数 BU を持つ企業」を想定して出題される
- 「Parent BU からのみ実施できる作業」「子 BU に反映される設定」「BU を跨ぐデータ共有」の論点は頻出
- 一斉送信を想定する問題と、API 駆動の 1 件単位送信を想定する問題が混在する

## Term Tooltips to Add in Section 0

以下の用語は初出時にツールチップを付与する:

- **BU**: Business Unit。Marketing Cloud 内の組織分割単位。Enterprise 2.0 構成では Parent BU の下に複数の Child BU を持てる。
- **Contact Builder**: Marketing Cloud のアプリの 1 つ。Contact Model、Population、Attribute Group を管理する場所。
- **Sendable DE**: 送信対象として利用可能な Data Extension。Subscriber Relationship 設定が必須。
- **Enterprise 2.0**: 複数の BU を持てる Marketing Cloud のアカウント構造。

## Mobile Responsiveness

### Tables on Mobile
- 表は .table-wrap で横スクロール可能
- フォントサイズはモバイルで 0.85rem に縮小

### Tooltips on Mobile
- 768px 以下では tap で開閉
- 開いたツールチップが画面外にはみ出さないよう、必要なら max-width: 80vw

### Copy Button on Mobile
- ボタンは見えるが、ラベル「Copy」は非表示にしてアイコンのみ表示（width 縮小）
- もしくは現状の表示維持で OK（タップ可能なサイズは保つ）

## Out of Scope (This Change)
- 他のページ（page-macro 以降）への変更
- §0 以外のコンテンツ追加
- 既存の TOP ページの変更

## Quality Checklist
実装後、以下を確認:
- [ ] page-prereq の data-placeholder 属性を削除
- [ ] page-prereq に上記コンテンツが入っている
- [ ] 表の横スクロールがモバイル幅で動作する
- [ ] 用語ツールチップが PC でホバー、モバイルでタップで表示される
- [ ] コピーボタンが各セクション h2 の右に表示される
- [ ] コピーボタンクリックで Markdown 形式でクリップボードに入る
- [ ] コピー成功時に「Copied」表示が出る（1.5秒で戻る）
- [ ] 他のページ（page-top など）が引き続き正常動作する
- [ ] ダークトーンのデザイン一貫性が保たれている