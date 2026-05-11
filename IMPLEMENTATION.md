# Initial Implementation Spec for index.html

## Goal
このプロジェクトの index.html 初回バージョンを作成する。
TOPページのコンテンツを完全実装し、他のページは枠とプレースホルダーのみ用意する。
UI とデザインの土台確認用バージョン。

## File Structure
- 単一の index.html ファイルにすべてを内包
- 外部依存なし（Webfont含めて外部リソースは使わない）

## HTML Head Requirements
- charset UTF-8
- viewport: width=device-width, initial-scale=1
- title: "MC-202 試験対策ガイド"
- meta name="last-updated" content="2026/05/11" （これを更新日として JS で読み取る）
- meta name="robots" content="noindex, nofollow" （検索エンジン除外）
- favicon: 不要

## Page Structure
- 左側：固定サイドバー（幅 260px）
- 右側：メインコンテンツエリア
- モバイル（768px未満）：サイドバーは閉じた状態がデフォルト、ハンバーガーボタンで開閉

## Sidebar Content
タイトル「MC-202 試験対策ガイド」
ナビゲーション項目（クリックでメインエリアの該当ページに切替）:
- TOP
- 0. 前提知識
- 1. マクロ
- 2. メソ（展開可能、サブ項目: Best Practices / Content / Automation / Subscriber & Data / Analytics）
- 3. ミクロ
- 4. ひっかけ集
- 5. チートシート
- 6. 学習ロードマップ
- 7. 練習問題70問（展開可能、サブ項目: Section 1 / Section 2 / Section 3 / Section 4 / Section 5 / 模試）

サブ項目は親項目クリックで展開/折りたたみ可能。
現在表示中のページがハイライトされる。

## TOP Page Content (完全実装)

### 1. ヘッダーセクション
タイトル: "MC-202 試験対策ガイド"
サブタイトル: "Salesforce Marketing Cloud Email Specialist"

### 2. 注意喚起ボックス
内容:
"試験傾向や試験情報は日々アップデートされます。公式でも注意喚起がされています。
情報鮮度を確認のうえ、必要な行動を取ってください。
旧情報を元に学習すると逆効果なのでご注意ください。
公式 Trailhead は直近の試験傾向を反映しているわけではないので、鮮度ある情報とハイブリッドで活用してください。"

デザイン: 落ち着いた背景色（少し明るめの surface）、左に細いボーダーライン、過度な装飾なし

### 3. メタ情報ボックス
4 行で表示:
- 更新日: yyyy/mm/dd （HTML meta tag から JS で取得）
- 情報鮮度: ステータス（動的判定）
- 推奨行動: ステータスに応じたメッセージ
- 更新者: Yukihiro Murai

### 4. 情報鮮度の判定ロジック
JavaScript で以下を計算:
```javascript
const updatedDate = new Date(document.querySelector('meta[name="last-updated"]').content.replace(/\//g, '-'));
const now = new Date();
const monthsElapsed = (now - updatedDate) / (1000 * 60 * 60 * 24 * 30.44);

if (monthsElapsed <= 3) {
  freshness = '良好';
  action = '必要な箇所は調べながら学習を進める';
  statusClass = 'fresh-good';
} else if (monthsElapsed <= 6) {
  freshness = '通常';
  action = '最新情報が欲しい場合のみ更新者へ連絡';
  statusClass = 'fresh-normal';
} else {
  freshness = '不良';
  action = '最新情報への更新が必要なので更新者へ連絡';
  statusClass = 'fresh-stale';
}
```

ステータスの色分け（控えめなドット表示）:
- fresh-good: ミュートな緑
- fresh-normal: ミュートな琥珀
- fresh-stale: ミュートな赤

### 5. 連絡フォーム
タイトル: "更新者へ連絡"
要素:
- textarea（行数 4-5、placeholder: "更新依頼・お問い合わせ内容をご記入ください"）
- 送信ボタン: クリックで mailto: を開く
  - mailto アドレス: yukihiro-murai@bigm2y.com
  - 件名プレフィル: "[MC-202ガイド] 更新依頼・お問い合わせ"
  - 本文プレフィル: textarea の内容を URL エンコードして mailto の body= に渡す

## Other Pages (プレースホルダー)
TOP 以外の各ページは以下のプレースホルダー:
- 見出し（章タイトル）
- "Coming soon — このページのコンテンツは順次追加されます" の表示
- スマートなプレースホルダーカード 1 つ

## Design System

### Color Variables (CSS Variables)
```css
:root {
  --bg-base: #0a0a0a;
  --bg-surface: #171717;
  --bg-elevated: #1f1f1f;
  --border-subtle: #262626;
  --border-default: #333333;
  --text-primary: #fafafa;
  --text-secondary: #a1a1aa;
  --text-tertiary: #71717a;
  --accent: #5eb8a0;
  --accent-hover: #6fc9b1;
  --accent-muted: rgba(94, 184, 160, 0.1);
  --status-good: #5eb8a0;
  --status-normal: #d4a574;
  --status-stale: #c87171;
}
```

### Typography
- font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Hiragino Sans", "Hiragino Kaku Gothic ProN", "Yu Gothic", "Meiryo", sans-serif
- base font-size: 15px
- line-height: 1.7
- 見出しは少し詰めて line-height: 1.4

### Spacing
- セクション間: 48px
- ブロック間: 24px
- 内側余白: 16-24px

### Borders & Radius
- border-radius: 6px（控えめ）
- border-width: 1px

### Component Patterns
- ボタン: 背景透明 + 細いボーダー + ホバーで accent カラー
- カード: bg-surface + 細いボーダー + 適度な余白
- リンク: accent カラー、下線なし、ホバーで accent-hover

## JavaScript Functionality

### 1. Page Routing (シンプルな SPA 風)
URLハッシュベースのルーティング:
- `#top` → TOPページ
- `#prereq` → 前提知識
- `#macro` → マクロ
- `#meso` → メソ
- `#meso-bestpractices` → メソ Best Practices
- `#meso-content` → メソ Content
- `#meso-automation` → メソ Automation
- `#meso-subscriber` → メソ Subscriber & Data
- `#meso-analytics` → メソ Analytics
- `#micro` → ミクロ
- `#traps` → ひっかけ集
- `#cheatsheet` → チートシート
- `#roadmap` → 学習ロードマップ
- `#quiz-section1` から `#quiz-mock` まで → 各クイズセクション

ハッシュなしでアクセスした場合は TOP を表示。

### 2. Freshness Calculation
ページロード時に情報鮮度を計算して表示。

### 3. Sidebar Toggle (Mobile)
ハンバーガーボタンクリックでサイドバーをスライドイン/アウト。
オーバーレイ表示（背景クリックで閉じる）。

### 4. Active Navigation Highlight
現在表示中のページに対応するサイドバー項目に accent カラーを適用。

### 5. Submenu Expansion
「2. メソ」「7. 練習問題70問」をクリックで展開/折りたたみ。
サブ項目クリックで該当ページ表示。

### 6. Mailto Form
送信ボタンクリックで:
```javascript
const body = encodeURIComponent(document.querySelector('#contact-textarea').value);
window.location.href = `mailto:yukihiro-murai@bigm2y.com?subject=${encodeURIComponent('[MC-202ガイド] 更新依頼・お問い合わせ')}&body=${body}`;
```

## Responsive Breakpoints
- Desktop: 769px 以上 → サイドバー常時表示
- Mobile: 768px 以下 → サイドバー初期非表示、ハンバーガーで開閉

## Code Quality
- HTML: semantic markup
- CSS: CSS Variables 主体
- JavaScript: グローバル汚染なし。IIFE で囲む
- インデント: スペース 2 文字
- コメント: 各セクションの目的を 1 行コメント

## Out of Scope (今回は実装しない)
- 各章の本コンテンツ（プレースホルダーのみ）
- クイズ機能（プレースホルダーのみ）
- Markdown コピー機能（後で追加）
- 用語ツールチップ（後で追加）