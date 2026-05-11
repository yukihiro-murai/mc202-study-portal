# Section 6: 3ヶ月学習ロードマップ — Implementation Spec

## Goal
index.html の「6. 学習ロードマップ」ページ (id="page-roadmap") のプレースホルダーを削除し、本コンテンツを実装する。
あくまで参考プランとして提示する（メンバーの状況による調整前提）。

## Target File
index.html (単一ファイル維持)

## Scope of This Change
- page-roadmap のコンテンツ実装
- 新規 UI パターンの追加は不要（既存の表、チェックリスト、強調を再利用）

## Page: §6 学習ロードマップ — page-roadmap

### Page Header
```html
<header class="page__header">
  <p class="page__subtitle">SECTION 6</p>
  <h1 class="page__title">3 ヶ月学習ロードマップ</h1>
</header>
```

### Lead
> 参考プラン。メンバーの稼働状況により調整してください。
> 前提：MC-101 Foundations の Trailmix 完了済み、実機環境なし。

### Block: 全体方針

ul:
- <em class="em">1 ヶ月目</em>: 全体像と用語の地図づくり。試験範囲を「見たことがある」状態にする
- <em class="em">2 ヶ月目</em>: 判断軸の獲得。シナリオベースで「何を選ぶか」を即答できる状態にする
- <em class="em">3 ヶ月目</em>: 弱点補強と模試周回。85% 安定で本番に臨む

### Block: Month 1 — 地図づくり（Week 1-4）

table (data-table):

| 週 | 学習内容 | 推奨時間 |
|---|---|---|
| W1 | 本ガイド §0 前提知識・§1 マクロ通読 / 公式 Trailmix「Prepare for your MC Email Specialist Credential」開始 | 6-8h |
| W2 | §2.1 Best Practices + §2.2 Content / Trailmix の Email Studio・Content Builder モジュール | 6-8h |
| W3 | §2.3 Marketing Automation / Trailmix の Automation Studio・Journey Builder モジュール | 6-8h |
| W4 | §2.4 Subscriber and Data Management + §2.5 Analytics / Trailmix 完走 | 8-10h |

#### Month 1 終了時のチェックポイント

interactive checklist:
- 5 セクションの配点を即答できる
- Automation Studio / Journey Builder / Triggered Send の使い分けを言える
- Subscriber Key / Contact Key / Primary Key の違いを説明できる
- Send Classification の 3 要素を言える

### Block: Month 2 — 判断軸の獲得（Week 5-8）

table:

| 週 | 学習内容 | 推奨時間 |
|---|---|---|
| W5 | §3 ミクロのシナリオ 15 本を 1 日 2-3 本ペースで解く / 各シナリオの判断軸を自分の言葉でメモ化 | 6-8h |
| W6 | §4 高頻度ひっかけ集の通読 / §5 チートシート暗記 / 練習問題 Section 1-2 (20 問) を解く | 8-10h |
| W7 | 練習問題 Section 3-4 (20 問) を解く / 誤答を §2 に戻って復習 | 8-10h |
| W8 | 練習問題 Section 5 + 弱点セクションの追加学習 / Salesforce Help を Send Classification・Data Extension・Send Logging で読み込む | 8-10h |

#### Month 2 終了時のチェックポイント

interactive checklist:
- §1.5 の混同しやすい組み合わせを全て即答できる
- §4.1-4.5 のひっかけ表を全て即答できる
- 送信差異の原因を 10 個以上挙げられる
- AMPscript の Lookup / AttributeValue / IIF の基本構文が読める

### Block: Month 3 — 模試周回と弱点補強（Week 9-12）

table:

| 週 | 学習内容 | 推奨時間 |
|---|---|---|
| W9 | 練習問題 模試 20 問（時間計測） / 誤答セクションを集中復習 / 公式 Trailhead Maintenance (Spring '26) モジュール | 8-10h |
| W10 | 外部模試（FocusOnForce / SalesforceExams など）を 1 セット / 誤答パターン分析 | 10-12h |
| W11 | 外部模試 2 セット目 / 65% 以上を 3 回連続で取れるまで反復 / Send Discrepancy 12 項目を完全暗記 | 10-12h |
| W12 | 練習問題 70 問を全周回（時間配分意識） / §5 チートシート最終確認 / 試験前々日は休息、前日は軽い確認のみ | 8-10h |

#### Month 3 終了時のチェックポイント（合格ライン感覚）

interactive checklist:
- Sendable DE と Non-Sendable DE の違いを 30 秒で説明できる
- Subscriber Key と Primary Key の違いを即答できる
- Publication List と Suppression List の違いを即答できる
- 送信予定数より実送信数が少ない原因を 10 個以上挙げられる
- Automation Studio と Journey Builder の境界を即答できる
- SQL Query と Data Filter の選択基準を即答できる
- Triggered Send を使うべき場面を 3 パターン挙げられる
- Sender Profile / Delivery Profile / Send Classification の違いを即答できる
- Data Extract 後に SFTP へ出すには何が必要かを即答できる
- CTR と CTOR の違いを即答できる
- Send Logging と Tracking Extract の違いを即答できる
- Einstein STO / Engagement Scoring / Content Selection の違いを即答できる
- 模試で 85% 以上を 3 回連続で取れている

### Block: 短縮プラン

リード:
> 業務多忙で 3 ヶ月確保が難しい場合の参考短縮版。

ul:
- <em class="em">1 ヶ月圧縮版</em>: §0-2 を 1 週間で通読 → §3-5 を 2 週目で暗記 → 練習問題 70 問を 3 週目で 2 周 → 4 週目で外部模試。推奨: すでに実務経験あるメンバー向け
- <em class="em">2 ヶ月版</em>: Month 1 を 3 週、Month 2 を 3 週、Month 3 を 2 週に圧縮

## Quality Checklist

- [ ] page-roadmap の data-placeholder 属性を削除
- [ ] 全体方針、Month 1〜3 の月別表、終了時チェックポイント、短縮プランが順に表示される
- [ ] 各月の終了時チェックポイントが §5 と同じインタラクティブチェックリスト形式
- [ ] チェックリストはリロードでリセット（localStorage 使わない）
- [ ] 各 h2 にコピーボタンがある
- [ ] 強調 (.em) がアクセントカラーで表示される
- [ ] 他のページが引き続き正常動作する

## Out of Scope
- 他のページへの変更
- §6 以外のコンテンツ追加
- 既存ページの変更