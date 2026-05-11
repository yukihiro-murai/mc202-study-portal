# Section 4: 高頻度ひっかけ集約版 — Implementation Spec

## Goal
index.html の「4. ひっかけ集」ページ (id="page-traps") のプレースホルダーを削除し、本コンテンツを実装する。
試験直前 2-3 日の暗記用ページ。モバイルでの可読性を最優先する。

5 つのテーマ別グループ（識別子・キー系 / 購読・除外系 / 送信設定系 / Automation・Journey 系 / Analytics 系）に「罠 / 正しい理解」のペアをまとめる。

## Target File
index.html (単一ファイル維持)

## Scope of This Change
- page-traps のコンテンツ実装
- ひっかけペアカード (.trap-pair) の共通スタイル追加
- レスポンシブ切替: PC では 2 列の表、モバイルではカード縦並び
- それ以外のページへの変更は不要

## Common UI Pattern to Establish

### Trap Pair (ひっかけペア)
PC では 2 列のシンプルな表、モバイル (≤ 768px) では「罠」と「正しい理解」が縦並びのカード形式に切り替わる。

### Desktop（≥ 769px）: テーブル表現
既存の .data-table をそのまま使う。

```html
<div class="table-wrap">
  <table class="data-table trap-table">
    <thead>
      <tr>
        <th>罠</th>
        <th>正しい理解</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>Email Address を一意 ID にする</td>
        <td>Subscriber Key / Contact Key を使う</td>
      </tr>
      <!-- ... -->
    </tbody>
  </table>
</div>
```

### Mobile（≤ 768px）: カード形式
同じ HTML を CSS だけで見た目を切り替える。テーブルの行をブロック化し、罠は赤系の左ボーダー、正しい理解は緑系（accent）の左ボーダーで視覚的に対比させる。

CSS:
```css
.trap-table {
  width: 100%;
}

/* Desktop: standard table */
.trap-table thead th:first-child,
.trap-table tbody td:first-child {
  width: 45%;
}

@media (max-width: 768px) {
  /* Hide table head */
  .trap-table thead {
    display: none;
  }

  /* Each row becomes a card */
  .trap-table,
  .trap-table tbody,
  .trap-table tr,
  .trap-table td {
    display: block;
    width: 100%;
  }

  .trap-table tr {
    background: var(--bg-surface);
    border: 1px solid var(--border-subtle);
    border-radius: var(--radius);
    margin-bottom: 12px;
    padding: 14px 16px;
  }

  .trap-table td {
    border: none;
    padding: 0;
    position: relative;
    padding-left: 80px;
    min-height: 24px;
    font-size: 0.95rem;
    line-height: 1.6;
  }

  .trap-table td:first-child {
    margin-bottom: 10px;
    padding-bottom: 10px;
    border-bottom: 1px dashed var(--border-subtle);
  }

  /* Inject labels using ::before */
  .trap-table td:first-child::before {
    content: "罠";
    position: absolute;
    left: 0;
    top: 0;
    color: var(--status-stale);
    font-size: 0.75rem;
    font-weight: 600;
    letter-spacing: 0.04em;
    padding: 2px 10px;
    border: 1px solid var(--status-stale);
    border-radius: var(--radius);
    line-height: 1.4;
    background: rgba(200, 113, 113, 0.08);
  }

  .trap-table td:last-child::before {
    content: "正解";
    position: absolute;
    left: 0;
    top: 0;
    color: var(--accent);
    font-size: 0.75rem;
    font-weight: 600;
    letter-spacing: 0.04em;
    padding: 2px 10px;
    border: 1px solid var(--accent);
    border-radius: var(--radius);
    line-height: 1.4;
    background: var(--accent-muted);
  }
}
```

備考:
- ::before で「罠」「正解」のラベルバッジを表現することで、HTML を増やさず CSS だけでカード表示にできる
- ラベルバッジは控えめなボーダー + 薄背景で、ビビッドではない
- 罠側は status-stale 系（落ち着いた赤）、正解側は accent（teal）

## Page: §4 ひっかけ集 — page-traps

### Page Header
```html
<header class="page__header">
  <p class="page__subtitle">SECTION 4</p>
  <h1 class="page__title">高頻度ひっかけ集約版</h1>
</header>
```

### Lead
> §2 のセクション別ひっかけ表をテーマ別にクロス整理。試験直前 2-3 日の暗記用。

### Block: 4.1 識別子・キー系

table (trap-table):

| 罠 | 正しい理解 |
|---|---|
| Email Address を一意 ID にする | Subscriber Key / Contact Key を使う |
| Primary Key と Subscriber Key を同一視 | 役割が違う |
| Subscriber Key と Contact Key は完全に同じもの | ベストプラクティスは同一値だが概念は別レイヤー |
| Data Extension は送信に常に使える | Sendable 設定 + Subscriber Relationship 定義が必要 |

### Block: 4.2 購読・除外系

table:

| 罠 | 正しい理解 |
|---|---|
| Publication List は除外用 | カテゴリ別購読管理 |
| Suppression List は購読カテゴリ管理 | 送信除外（do-not-contact） |
| Unsubscribe = DE から削除 | ステータス管理が正解。物理削除しない |
| Sendable DE から削除すれば Contact も消える | Contact 層は別。All Contacts には残る |
| Transactional は Unsubscribe を完全に無視してよい | 設定・法令・目的次第。Transactional でも乱用は不可 |

### Block: 4.3 送信設定系

table:

| 罠 | 正しい理解 |
|---|---|
| From Name は Delivery Profile | Sender Profile |
| Footer は Sender Profile | Delivery Profile |
| Commercial / Transactional は Sender Profile | Send Classification の CAN-SPAM 分類 |
| Send Classification は単なるラベル | Sender + Delivery + CAN-SPAM の組み合わせテンプレート |

### Block: 4.4 Automation / Journey 系

table:

| 罠 | 正しい理解 |
|---|---|
| SQL 加工を Journey 内でやる | Automation Studio の SQL Query Activity |
| 顧客行動分岐を Automation で組む | Journey Builder |
| Data Extract だけで SFTP 転送完了 | File Transfer Activity が必要 |
| Triggered Send を定期一斉送信に使う | イベント駆動・1 件単位向き |
| Journey の Re-entry はデフォルトで再入場可 | デフォルトは No re-entry |
| A/B Test Send と Path Optimizer は同じ | 前者は 1 回の送信内、後者は Journey 内のパス比較 |

### Block: 4.5 Analytics 系

table:

| 罠 | 正しい理解 |
|---|---|
| Total と Unique を混同 | Unique = 人単位、Total = 総数 |
| CTR と CTOR を混同 | CTOR の分母は Open |
| Open Rate を絶対視 | Apple MPP / プライバシー保護で過大計上の可能性 |
| 長期データは MC 内に無期限保持 | 180 日保持。長期分析は外部退避前提 |
| Extract は無制限にキュー可能 | 250 件上限 |
| Send Logging はデフォルトで使える | Salesforce サポートで事前有効化必要 |
| Tracking Extract = Send Logging | 前者は既存 Tracking 抽出、後者は送信時点記録 |

## Mobile Considerations

- 表のフォントサイズはモバイルで 0.95rem に拡大（暗記用の可読性優先）
- セクション間の余白も気持ち多めに（block の margin-bottom を維持）
- 罠/正解バッジは小さくして圧迫感を出さない（既出 CSS の通り 0.75rem）

## Quality Checklist

- [ ] page-traps の data-placeholder 属性を削除
- [ ] 5 つのテーマ別グループが順に表示される（4.1〜4.5）
- [ ] 各 h2 にコピーボタンがある
- [ ] PC 幅では 2 列のシンプルな表として表示される
- [ ] モバイル幅（768px 以下）では各行がカード形式に変わる
- [ ] モバイル時に「罠」「正解」のラベルバッジが各列の上に表示される
- [ ] 罠バッジは status-stale 系（控えめ赤）、正解バッジは accent（teal）
- [ ] スクロール時の視認性が高い
- [ ] 他のページが引き続き正常動作する

## Out of Scope
- 他のページへの変更
- §4 以外のコンテンツ追加
- 既存ページの変更