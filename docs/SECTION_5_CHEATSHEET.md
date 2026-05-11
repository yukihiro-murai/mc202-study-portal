# Section 5: 判断軸チートシート — Implementation Spec

## Goal
index.html の「5. チートシート」ページ (id="page-cheatsheet") のプレースホルダーを削除し、本コンテンツを実装する。
試験直前確認用、モバイルでの閲覧も最優先。

## Target File
index.html (単一ファイル維持)

## Scope of This Change
- page-cheatsheet のコンテンツ実装
- インタラクティブチェックリスト (.checklist) の共通スタイル + 軽量 JS 追加
- それ以外のページへの変更は不要

## Common UI Pattern to Establish

### Interactive Checklist
クリックでチェック可能なリスト。リロードで初期化（記録しない）。

HTML 構造:
```html
<ul class="checklist">
  <li class="checklist__item">
    <button class="checklist__toggle" aria-pressed="false" aria-label="チェック">
      <span class="checklist__box"></span>
    </button>
    <span class="checklist__text">All Subscribers のステータス（Unsubscribed / Held / Bounced）</span>
  </li>
  <!-- ... -->
</ul>
```

CSS:
```css
.checklist {
  list-style: none;
  margin: 0;
  padding: 0;
}

.checklist__item {
  display: flex;
  align-items: flex-start;
  gap: 12px;
  padding: 8px 0;
  border-bottom: 1px solid var(--border-subtle);
}

.checklist__item:last-child {
  border-bottom: none;
}

.checklist__toggle {
  background: transparent;
  border: none;
  padding: 0;
  cursor: pointer;
  flex-shrink: 0;
  margin-top: 4px;
}

.checklist__box {
  display: inline-block;
  width: 18px;
  height: 18px;
  border: 1.5px solid var(--border-default);
  border-radius: 4px;
  position: relative;
  transition: border-color var(--transition), background var(--transition);
}

.checklist__toggle:hover .checklist__box {
  border-color: var(--accent);
}

.checklist__toggle[aria-pressed="true"] .checklist__box {
  background: var(--accent);
  border-color: var(--accent);
}

.checklist__toggle[aria-pressed="true"] .checklist__box::after {
  content: "";
  position: absolute;
  left: 4px;
  top: 0px;
  width: 5px;
  height: 10px;
  border-right: 2px solid var(--bg-base);
  border-bottom: 2px solid var(--bg-base);
  transform: rotate(45deg);
}

.checklist__text {
  flex: 1;
  color: var(--text-primary);
  line-height: 1.7;
  font-size: 0.95rem;
  transition: color var(--transition), text-decoration var(--transition);
}

.checklist__toggle[aria-pressed="true"] ~ .checklist__text {
  color: var(--text-tertiary);
  text-decoration: line-through;
}
```

JS:
```javascript
function bindChecklists() {
  document.querySelectorAll('.checklist__toggle').forEach(function (btn) {
    btn.addEventListener('click', function () {
      var pressed = btn.getAttribute('aria-pressed') === 'true';
      btn.setAttribute('aria-pressed', String(!pressed));
    });
  });
}
```

注意:
- リロードで初期化される（localStorage は使わない）
- チェック済みアイテムは文字色を薄くして取り消し線
- ボタンは独立した button 要素なので、キーボード操作も可能

## Page: §5 チートシート — page-cheatsheet

### Page Header
```html
<header class="page__header">
  <p class="page__subtitle">SECTION 5</p>
  <h1 class="page__title">判断軸チートシート</h1>
</header>
```

### Lead
> 試験直前確認用の判断軸まとめ。1 ブロック = 1 テーマで順次見られる構成。

### Block: 5.1 ツール選択フローチャート

pre タグ (ASCII):
```
顧客行動に応じた分岐・待機がある？        → YES → Journey Builder

定期バッチ処理 / ファイル取込 / SQL / SFTP？  → YES → Automation Studio

外部イベント駆動の即時 1 通送信？             → YES → Triggered Send

単発で手動送信したい？                       → YES → Guided Send

再利用可能な送信定義を作りたい？             → YES → User-Initiated Send
```

### Block: 5.2 機能境界マトリクス

table (data-table):

| 軸 A | 軸 B | 決め手 |
|---|---|---|
| Automation Studio | Journey Builder | バッチ処理 vs 顧客体験 |
| SQL Query | Data Filter | 複雑 JOIN vs 単純条件 |
| Publication List | Suppression List | カテゴリ別購読管理 vs 送信除外 |
| Sender Profile | Delivery Profile | From 系 vs 配送系 |
| Triggered Send | User-Initiated Send | API / イベント vs 再利用可能定義 |
| Send Logging | Tracking Extract | 送信時点記録 vs Tracking からの抽出 |
| Personalization String | AMPscript | 単純差込 vs 条件分岐 / 別 DE 参照 |
| Dynamic Content | AMPscript | GUI 管理 vs 開発者制御 |
| All Contacts | All Subscribers | Enterprise 横断 / 全チャネル vs BU 内 Email 購読者 |
| Contact Key | Subscriber Key | チャネル横断 ID vs Email Studio 識別子 |
| A/B Test Send | Path Optimizer | Email Studio 内 vs Journey Builder 内 |
| Shared DE | Local DE | 親 BU 作成・子 BU 参照 vs 単一 BU |

### Block: 5.3 Send Classification 三層構造

pre タグ (ASCII):
```
┌────────────────────────────────────────────┐
│         Send Classification                │
│  ┌──────────────────────────────────────┐  │
│  │ Sender Profile                       │  │
│  │  • From Name / From Email            │  │
│  │  • Reply Mail Management (RMM)       │  │
│  └──────────────────────────────────────┘  │
│  ┌──────────────────────────────────────┐  │
│  │ Delivery Profile                     │  │
│  │  • IP                                │  │
│  │  • Header / Footer                   │  │
│  │  • Domain                            │  │
│  └──────────────────────────────────────┘  │
│  ┌──────────────────────────────────────┐  │
│  │ CAN-SPAM Classification              │  │
│  │  • Commercial / Transactional        │  │
│  └──────────────────────────────────────┘  │
└────────────────────────────────────────────┘
```

### Block: 5.4 Contact Identity 階層

pre タグ (ASCII):
```
[Contact ID]                    ← システム内部数値（不変・通常意識せず）
   │
[Contact Key]                   ← Contact Builder で使用、チャネル横断
   │  (ベストプラクティス：= Subscriber Key)
   │
[Subscriber Key]                ← Email Studio で使用、Email 購読者識別
   │
[Sendable DE の Primary Key]    ← DE 内の行を一意化（Subscriber Key と別物可）
```

### Block: 5.5 送信差異の原因チェックリスト

リード:
> 送信予定数より実送信数が少ない場合、上から順に確認する。

interactive checklist:
- All Subscribers のステータス（Unsubscribed / Held / Bounced）
- Publication List のオプトアウト
- Suppression List 該当
- Exclusion List / Exclusion Script
- Sendable DE の Subscriber Relationship 設定
- Subscriber Key の重複・不一致
- Email Address の空・無効
- SQL / Filter の抽出条件
- Send Classification の Commercial / Transactional 設定
- ドメインブロック / List Detective
- Journey re-entry / exit criteria
- Frequency Cap / STO 影響

### Block: 5.6 Einstein 機能の 3 秒判別

table:

| 問題文のキーワード | 機能 |
|---|---|
| 「送信時刻」「タイミング」 | Send Time Optimization (STO) |
| 「予測」「離脱可能性」「スコア」 | Engagement Scoring |
| 「頻度」「送りすぎ」「少なすぎ」 | Engagement Frequency |
| 「コンテンツの自動選択」 | Content Selection |
| 「件名生成」「多言語」「ローカライズ」 | Generative AI / Copy Insights |

### Block: 5.7 データ手段の使い分け

table:

| 用途 | 手段 |
|---|---|
| リアルタイムで送信履歴を見たい | Data Views（`_Sent`、`_Open` 等） |
| 送信時点の属性を残したい | Send Logging |
| Tracking データを外部にファイル出力 | Tracking Extract |
| Journey 単位の分析 | Journey Analytics |
| BU 横断・多次元分析 | Marketing Cloud Intelligence |

## Mobile Considerations

- pre タグの ASCII 図はモバイルでは横スクロール可（既存スタイル）
- チェックリストはモバイルでも十分タップしやすい大きさ（ボックス 18px、タップ領域は親 button で確保）
- table もモバイルで横スクロール可（既存）
- セクション間の余白を維持

## Quality Checklist

- [ ] page-cheatsheet の data-placeholder 属性を削除
- [ ] 7 つのブロック（5.1〜5.7）が順に表示される
- [ ] 各 h2 にコピーボタンがある
- [ ] 5.1, 5.3, 5.4 の ASCII 図が monospace で表示される
- [ ] 5.2, 5.6, 5.7 の表が正しく表示される
- [ ] 5.5 のチェックリストでクリックすると チェックが入る / 外れる
- [ ] チェック済みアイテムは色が薄くなり取り消し線が引かれる
- [ ] リロードするとチェックがリセットされる（localStorage 使わない）
- [ ] モバイル幅でも全要素が読みやすい
- [ ] 他のページが引き続き正常動作する

## Out of Scope
- 他のページへの変更
- §5 以外のコンテンツ追加
- 既存ページの変更
- チェック状態の永続化