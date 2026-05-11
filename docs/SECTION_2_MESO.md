# Section 2: メソ（セクション別出題ポイント） — Implementation Spec

## Goal
index.html の「2. メソ」配下の 6 ページ（page-meso, page-meso-bestpractices, page-meso-content, page-meso-automation, page-meso-subscriber, page-meso-analytics）のプレースホルダーを削除し、本コンテンツを実装する。

- page-meso: §2 全体の導入ページ（5サブセクションへの案内）
- page-meso-bestpractices: §2.1 Email Marketing Best Practices
- page-meso-content: §2.2 Content Creation and Delivery
- page-meso-automation: §2.3 Marketing Automation
- page-meso-subscriber: §2.4 Subscriber and Data Management
- page-meso-analytics: §2.5 Insights and Analytics

## Target File
index.html (単一ファイル維持)

## Scope of This Change
- §2 配下 6 ページのコンテンツ実装
- 「セクション索引カード」の共通スタイル追加（page-meso で使用）
- それ以外のページ（page-prereq、page-macro、page-micro 以降）への変更は不要

## Common UI Pattern to Establish

### Section Index Cards
page-meso（§2 全体の導入）で使用する、5 サブセクションへのリンクカード。

HTML 構造:
```html
<div class="section-index">
  <a class="section-index__card" href="#meso-bestpractices">
    <div class="section-index__badge">2.1</div>
    <div class="section-index__body">
      <h3 class="section-index__title">Email Marketing Best Practices</h3>
      <p class="section-index__weight">配点 10%</p>
      <p class="section-index__desc">メール設計、到達性、購読管理、法令対応</p>
    </div>
  </a>
  <!-- 他のカードも同様 -->
</div>
```

CSS:
```css
.section-index {
  display: grid;
  grid-template-columns: 1fr;
  gap: 12px;
}

.section-index__card {
  display: flex;
  gap: 16px;
  background: var(--bg-surface);
  border: 1px solid var(--border-subtle);
  border-radius: var(--radius);
  padding: 18px 20px;
  text-decoration: none;
  color: var(--text-primary);
  transition: border-color var(--transition), background var(--transition);
}

.section-index__card:hover {
  border-color: var(--accent);
  background: var(--bg-elevated);
}

.section-index__badge {
  background: var(--accent-muted);
  color: var(--accent);
  border-radius: var(--radius);
  width: 48px;
  height: 48px;
  flex-shrink: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1rem;
  font-weight: 600;
}

.section-index__body {
  flex: 1;
}

.section-index__title {
  margin: 0 0 4px;
  font-size: 1.05rem;
  color: var(--text-primary);
}

.section-index__weight {
  margin: 0 0 6px;
  font-size: 0.85rem;
  color: var(--accent);
}

.section-index__desc {
  margin: 0;
  font-size: 0.9rem;
  color: var(--text-primary);
}
```

## Page: §2 メソ（導入ページ） — page-meso

### Page Header
```html
<header class="page__header">
  <p class="page__subtitle">SECTION 2</p>
  <h1 class="page__title">メソ：セクション別の出題ポイント</h1>
</header>
```

### Lead Paragraph
> 試験の 5 セクションを順に深掘りする。配点と頻出論点、ひっかけパターンを各サブセクションで整理する。

### Section Index (5 cards)
1. 2.1 Email Marketing Best Practices / 配点 10% / "メール設計、到達性、購読管理、法令対応"
2. 2.2 Content Creation and Delivery / 配点 24% / "メール作成、コンテンツ管理、送信準備、送信設定"
3. 2.3 Marketing Automation / 配点 26% / "Automation Studio、Journey Builder、Triggered Send の選択"
4. 2.4 Subscriber and Data Management / 配点 26% / "Data Extension、Subscriber Key、購読管理、送信差異"
5. 2.5 Insights and Analytics / 配点 14% / "メール指標、レポート、Einstein 製品の選定"

カードクリックで該当ページに遷移する。

---

## Page: §2.1 Best Practices — page-meso-bestpractices

### Page Header
```html
<header class="page__header">
  <p class="page__subtitle">SECTION 2.1 — 配点 10%</p>
  <h1 class="page__title">Email Marketing Best Practices</h1>
</header>
```

### Block: 何を問うか
リード:
> メールマーケティングの設計・到達性・購読管理・法令対応を、顧客シナリオで判断できるか。

### Block: メールデザインの基本
リード:
> HTML/CSS の細部より、どの設計が<em class="em">受信者体験・到達性・効果測定に適しているか</em>を問われる。

押さえる点（ul）:
- モバイル対応、レスポンシブ、画像依存の回避
- 明確な CTA、Subject Line / Preheader / From Name
- アクセシビリティ
- テキストと画像のバランス
- パーソナライズの過不足

### Block: 到達性（Deliverability）
押さえる用語（ul）:
- SPF<sup>*</sup> / DKIM<sup>*</sup> / DMARC<sup>*</sup>
- Sender Authentication Package (SAP)
- Dedicated IP、IP warming、Sender reputation
- Bounce、Complaint、Spam trap
- List hygiene、Engagement

表「典型判断」:

| シナリオ | 判断 |
|---|---|
| 新規専用 IP で大量送信したい | IP warming が必要 |
| 開封・クリックが低く苦情が多い | 配信対象の品質・頻度・同意を見直す |
| 到達率が悪い | 認証 (SPF/DKIM/DMARC)、レピュテーション、バウンス、苦情率を見る |
| 古いリストに一斉送信したい | リスト品質と同意がリスク |
| 専用 IP は常に良い？ | 送信量・warming・運用品質次第 |

### Block: 法令・同意・購読管理
リード:
> 試験では条文の暗記ではなく、<em class="em">Marketing Cloud 上でどの設定・運用で対応するか</em>が問われる。

押さえる点（ul）:
- CAN-SPAM、GDPR、日本の特定電子メール法
- Commercial vs Transactional
- Unsubscribe、物理住所、Preference Center
- Publication List、Suppression List
- 同意取得、再許諾、購読者保持

### Block: ひっかけ

表:

| 罠 | 正しい理解 |
|---|---|
| Transactional なら何を送ってもよい | 取引目的のメールである必要がある |
| Unsubscribe を削除で処理する | 削除ではなく購読状態・除外設計で扱う |
| 到達率はメール本文だけで決まる | 認証、送信者評価、リスト品質、エンゲージメントも影響 |
| Dedicated IP は常に良い | 送信量・warming・運用品質次第 |

---

## Page: §2.2 Content Creation and Delivery — page-meso-content

### Page Header
```html
<header class="page__header">
  <p class="page__subtitle">SECTION 2.2 — 配点 24%</p>
  <h1 class="page__title">Content Creation and Delivery</h1>
</header>
```

### Block: 何を問うか
> メール作成、コンテンツ管理、送信準備、送信設定。

### Block: 中核機能

表:

| 機能 | 役割 |
|---|---|
| Content Builder | コンテンツ作成・管理。Email / Block / Template を保持 |
| Email Studio | メール送信・購読者・Tracking |
| Template | 再利用可能な構造（HTML レベル） |
| Content Block | 再利用可能な部品 |
| Dynamic Content | 条件に応じたコンテンツ切替（GUI） |
| Personalization String | 単純な差し込み（%%FirstName%%） |
| AMPscript<sup>*</sup> | 条件分岐・データ参照・高度なパーソナライズ |
| SSJS<sup>*</sup> | より柔軟な処理 |
| Approvals | 送信前承認ワークフロー |
| A/B Testing | 件名、差出人、本文などの比較 |
| Test Send | テスト配信 |
| Subscriber Preview | 購読者データを使った表示確認 |

### Block: AMPscript 必須知識
リード:
> 詳細構文より<em class="em">使い分けと関数選択</em>が問われるが、最低限の構文判別は普通に出る。

デリミタとブロック（pre タグ、monospace）:
```
%%[ ... ]%%        ← AMPscriptブロック（処理のみ、出力なし）
%%=v(@var)=%%      ← インライン出力
%%FirstName%%       ← Personalization String（DE/Subscriberから差込）
```

頻出関数の表:

| 関数 | 用途 |
|---|---|
| `AttributeValue("FirstName")` | Subscriber / DE 属性を取得 |
| `Lookup("DE名", "返したいカラム", "検索カラム", "値")` | DE から 1 件取得 |
| `LookupRows(...)` | DE から複数行取得（Row 関数で展開） |
| `Field(row, "カラム")` | LookupRows の結果から値抽出 |
| `IIF(条件, 真値, 偽値)` | 三項演算 |
| `IF / ELSE / ENDIF` | 条件分岐 |
| `ProperCase / UpperCase / LowerCase` | 文字列加工 |

### Block: Personalization 手段の使い分け

表:

| 要件 | 選ぶもの |
|---|---|
| FirstName を差し込む | Personalization String |
| 属性によりバナーを変える（GUI） | Dynamic Content |
| DE を参照して条件分岐する | AMPscript |
| 複雑な処理をサーバー側で行う | SSJS |
| マーケターが GUI で管理したい | Dynamic Content |
| 開発者が柔軟に制御したい | AMPscript |

### Block: 送信方式の使い分け

表:

| 方式 | 向く場面 |
|---|---|
| Guided Send (Send Flow) | 手動で単発送信したい |
| User-Initiated Send | 再利用可能な送信定義を作りたい |
| Triggered Send | 外部イベント・API で 1 通ずつ送信したい |
| Transactional Send (TMS) | 高優先度の取引メール（API） |
| Journey Builder Email Activity | 顧客行動に応じた流れの中で送信したい |
| Automation Studio Send Email Activity | 定期バッチ処理の中で送信したい |

### Block: Triggered Send Definition のライフサイクル
ASCII 図:
```
Create (定義作成)
  → Verify (検証)
    → Start (アクティブ化)
      → (運用)
        → Pause / Stop (一時停止/停止)
```

注意:
> 試験では「Triggered Send が動かない」シナリオで Start し忘れ・Pause 状態が原因という問題がある。

### Block: Sender Profile / Delivery Profile / Send Classification
頻出。3 つを混同すると即失点。

表:

| 機能 | 定義するもの | 判断キーワード |
|---|---|---|
| Sender Profile | From Name / From Email / Reply Mail Management (RMM<sup>*</sup>) | 誰から送るか、返信処理 |
| Delivery Profile | IP、ヘッダー、フッター、Domain 設定など | どう配送するか |
| Send Classification | Sender Profile + Delivery Profile + CAN-SPAM 分類 (Commercial / Transactional) | 送信分類テンプレート |

重要結論ブロック (.takeaway):
> RMM は Sender Profile の一部。返信メールの自動処理（Out-of-Office の除外、Bounce 処理、Unsubscribe 処理）を含む。

### Block: A/B Testing
押さえる項目（ul）:
- Subject Line / From Name / Preheader / Email Content / Send Time
- Winner Criteria（Open Rate / Click Rate / CTOR）
- テスト対象割合 / Winner 送信対象割合

Path Optimizer 補足:
> <em class="em">Path Optimizer</em>: Journey Builder 内で複数パスを A/B テストし、勝者パスに残りの購読者を送る機能。Email Studio の A/B Testing とは別物。

### Block: ひっかけ

表:

| 罠 | 正しい理解 |
|---|---|
| 開封率を見たいのにクリック率を Winner 基準にする | 指標と目的を合わせる |
| From Name のテストに Sender Profile を意識しない | From 系は Sender Profile と関連 |
| 本文比較なのに件名だけ変える | 何を比較するかを明確にする |
| Footer 変更を Sender Profile で行う | Delivery Profile |
| Commercial / Transactional を Sender Profile で設定 | Send Classification（CAN-SPAM 分類） |

---

## Page: §2.3 Marketing Automation — page-meso-automation

### Page Header
```html
<header class="page__header">
  <p class="page__subtitle">SECTION 2.3 — 配点 26%</p>
  <h1 class="page__title">Marketing Automation</h1>
</header>
```

### Block: 何を問うか
> Automation Studio、Journey Builder、Triggered Send などを、顧客要件に応じて選べるか。

### Block: 3 大判断軸

表:

| 要件 | 選ぶもの |
|---|---|
| 定期バッチ処理 | Automation Studio |
| 顧客行動に応じた分岐・待機・離脱 | Journey Builder |
| API / イベントで即時 1 通送信 | Triggered Send / Transactional Send |
| ファイルを取り込んで対象者作成 | Automation Studio |
| SQL で複数 DE を加工 | Automation Studio |
| 顧客のライフサイクル施策 | Journey Builder |
| Welcome / Nurture / Re-engagement | Journey Builder |
| 単発メール配信 | Guided / User-Initiated Send |

### Block: Automation Studio の重要 Activity

表:

| Activity | 役割 |
|---|---|
| Import | ファイルから DE へ取り込み |
| File Transfer | SFTP 上のファイル移動、暗号化 / 復号、解凍 |
| Data Extract | データをファイルとして抽出 |
| SQL Query | DE 間の抽出・結合・加工 |
| Filter | 条件による抽出 |
| Script | SSJS 処理 |
| Send Email | メール送信 |
| Wait | 待機 |
| Verification | 件数・条件の検証 |
| Fire Event | Journey 起動 |
| Refresh Group | Group / Filtered List 更新 |

### Block: Automation Studio の典型順序

ASCII 図:
```
File Drop / Schedule
  ↓
File Transfer (SFTP→Safehouse)
  ↓
Import (Safehouse→DE)
  ↓
SQL Query / Filter
  ↓
Verification
  ↓
Send Email
  ↓
Data Extract (DE→ファイル)
  ↓
File Transfer (Safehouse→SFTP)
```

重要結論ブロック (.takeaway):
> <em class="em">Data Extract</em> = データをファイル化する（Safehouse 上にファイル生成）<br>
> <em class="em">File Transfer</em> = ファイルを移動する（Safehouse ⇄ SFTP）<br>
> Extract しただけでは外部 SFTP には届かない。File Transfer が必要。

### Block: Journey Builder の重要要素

表:

| 要素 | 役割 |
|---|---|
| Entry Source | 誰が Journey に入るか |
| Data Extension Entry | DE を起点にする |
| API Event | 外部イベントを起点にする |
| Salesforce Data Event | Sales / Service Cloud の変更を起点 |
| Wait | 待機（時間 / 曜日 / 特定日付 / 属性日付） |
| Decision Split | 属性・条件で分岐 |
| Engagement Split | 開封・クリックなどで分岐 |
| Random Split | ランダム分岐（A/B 用途以外） |
| Path Optimizer | 内蔵 A/B テスト（勝者パスへ寄せる） |
| Goal | 目的達成の定義 |
| Exit Criteria | 離脱条件 |
| Re-entry Settings | 再入場可否（None / Re-entry only after exiting / Re-entry anytime） |

HTS 補足:
> <em class="em">HTS</em>（High-Throughput Sending）: 大規模配信時に Journey の Email Activity で利用できる高速送信モード。Spring '25 で正式化。

### Block: Automation Studio vs Journey Builder

表:

| 比較軸 | Automation Studio | Journey Builder |
|---|---|---|
| 得意領域 | バッチ処理、SQL、ファイル処理 | 顧客体験、分岐、待機、行動連動 |
| 起点 | スケジュール、ファイル、手動 | Entry Source、API、DE、SF Event |
| データ加工 | 強い（SQL Query Activity） | 弱い |
| 顧客単位の流れ | 弱い | 強い |
| レポート | 処理結果中心 | Journey Analytics |
| 典型問題 | 毎週対象者抽出して送信 | 開封しなければ再送、クリックしたら別分岐 |

### Block: ひっかけ

表:

| 罠 | 正しい理解 |
|---|---|
| すべて Journey Builder でやる | SQL・ファイル処理は Automation Studio |
| Data Extract だけで外部 SFTP に送れる | File Transfer が必要 |
| Filter と SQL は同じ | 複数 DE JOIN や複雑条件は SQL |
| Triggered Send は一斉配信向き | イベント駆動・1 件単位向き |
| Journey Goal は後から何となく分析するためのもの | 目的達成を設計時点で定義する |
| Re-entry 設定をデフォルトのまま使う | デフォルトは「再入場不可」 |

---

## Page: §2.4 Subscriber and Data Management — page-meso-subscriber

### Page Header
```html
<header class="page__header">
  <p class="page__subtitle">SECTION 2.4 — 配点 26%</p>
  <h1 class="page__title">Subscriber and Data Management</h1>
</header>
```

### Block: 何を問うか
重要結論ブロック (.takeaway):
> <em class="em">最重要セクション。</em> Data Extension、Subscriber Key、Import、Segmentation、購読管理、送信差異の原因調査。

### Block: Data Extension の基本

表:

| 種別 | 役割 |
|---|---|
| Standard DE | 通常のデータ保持 |
| Sendable DE | 送信対象として使える |
| Non-Sendable DE | 参照用データ |
| Filtered DE | 条件で抽出された DE |
| Shared DE | Parent BU に作成され子 BU 間で共有可能 |
| Local DE | 単一 BU 内の DE |
| Synchronized DE | Marketing Cloud Connect 経由で SF 同期 |

### Block: Sendable DE の核心
ASCII 図:
```
Sendable Data Extension
  ├─ EmailAddress
  ├─ CustomerID  ← この値を
  ├─ FirstName
  └─ Subscriber relationship
        → All Subscribers の Subscriber Key  ← この値と紐付ける
```

注意:
> ここを誤ると、送信対象 / 購読管理 / Tracking / Journey entry / 重複排除 / レポート / Unsubscribe 処理がすべて壊れる。

### Block: Primary Key / Subscriber Key / Contact Key / Contact ID

表:

| 概念 | 役割 | 混同しやすい点 |
|---|---|---|
| Primary Key | DE 内の行を一意にする | Subscriber Key と同じではない |
| Subscriber Key | Email Studio 側の購読者識別子 | Email Address と同じではない |
| Contact Key | Contact Builder 側の連絡先識別子 | Subscriber Key と揃えるのがベストプラクティス |
| Contact ID | システム内部の数値 ID | ユーザーは変更不可・通常意識しない |
| Email Address | 配信先アドレス | 個人識別子としては不十分（共有・変更・複数） |

重要結論ブロック (.takeaway):
> ベストプラクティス: Subscriber Key = Contact Key を同一値で運用。Email Address を使うと、メアド変更や家族での共有時に同一人物を別 Contact として扱ってしまう。

### Block: Lists vs Data Extensions

表:

| 要件 | 選ぶもの |
|---|---|
| 小規模・単純な購読者管理 | Lists |
| 大規模データ | Data Extensions |
| API 利用 | Data Extensions |
| Triggered Send | Data Extensions |
| 複数属性・購入履歴・リレーショナルデータ | Data Extensions |
| SQL で加工したい | Data Extensions |
| シンプルなニュースレターリスト | Lists でも可（ただし非推奨） |

注意:
> 「大規模」「複数属性」「リレーショナル」「API」「Triggered Send」が出たら Data Extension をほぼ確定で疑う。

### Block: All Subscribers と Subscriber Status

表（4ステータス）:

| Status | 意味 | 試験上の注意 |
|---|---|---|
| Active | 送信可能 | 通常送信対象 |
| Bounced | バウンスあり | Hard Bounce で status 変更されることがある |
| Held | 配信不能状態 | バウンス閾値超過、無効アドレス等。送信差異の主要原因 |
| Unsubscribed | 購読解除 | 商用メール送信不可 |

### Block: Unsubscribe の 3 階層

表:

| 階層 | 適用範囲 |
|---|---|
| List-Level Unsubscribe | 特定のリスト / Publication List からのみ購読解除 |
| Channel-Level (BU-Level) Unsubscribe | その BU の全送信から購読解除 |
| Global Unsubscribe | アカウント全体の全送信から購読解除（CAN-SPAM 対応の最終手段） |

DoNotTrack 補足:
> <em class="em">DoNotTrack Attribute</em>: 特定購読者の Open / Click を記録しない設定。社員テストや個別プライバシー要請対応に使用。

### Block: Publication / Suppression / Exclusion

表:

| 種別 | 用途 | 判断キーワード |
|---|---|---|
| Publication List | メール種別ごとの購読管理 | ニュースレター、プロモ、製品アップデート |
| Suppression List | 送ってはいけない人を除外 | Do not contact、苦情、法令対応 |
| Exclusion List | 特定送信から除外 | このキャンペーンだけ除外 |
| Exclusion Script | 条件式で動的除外 | 複雑な除外条件 |

### Block: Segmentation の使い分け

表:

| 要件 | 選ぶもの |
|---|---|
| 単純条件で抽出 | Data Filter / Filtered DE |
| 複数 DE を JOIN | SQL Query |
| 動的な日付条件 | SQL Query |
| マーケターが GUI で管理 | Data Filter |
| 定期的に対象者更新 | Automation Studio + SQL / Filter |
| Journey 投入用に対象者を作る | Sendable DE + Journey Entry |

### Block: Send Discrepancy ：送信数差異の原因（最頻出）
リード:
> 「送信予定数より実際の送信数が少ない」場合、以下を<em class="em">順番に疑う</em>。

表:

| 優先度 | 原因 |
|---|---|
| 1 | All Subscribers で Unsubscribed / Held / Bounced |
| 2 | Publication List で対象カテゴリを unsubscribe |
| 3 | Suppression List に入っている |
| 4 | Exclusion List / Exclusion Script で除外 |
| 5 | Sendable DE の Subscriber relationship が誤っている |
| 6 | Subscriber Key が重複・不一致 |
| 7 | Email Address が空・無効・重複 |
| 8 | Data Extension の対象抽出条件が誤っている |
| 9 | SQL / Filter の結果が想定と違う |
| 10 | Send Classification の Commercial / Transactional 設定 |
| 11 | ドメインブロック、List Detective、到達性制限 |
| 12 | Journey re-entry / exit criteria により入っていない |
| 13 | Frequency Cap / Send-Time Optimization の影響 |

### Block: Contact Builder & Contact Model

表:

| 要素 | 役割 |
|---|---|
| Data Designer | Contact Builder 内でデータモデルを定義するツール |
| Attribute Group | 関連する DE をまとめた「ミニデータモデル」 |
| Attribute Set | Attribute Group 内の個別属性集合 |
| Population | 識別子の異なる Contact 群（例：顧客 ID vs 従業員 ID）。3 つ以下に抑えるのがベストプラクティス。Population 自体は Sendable ではない |
| Contact Deletion | Sendable DE から消しても All Contacts からは消えない。Contact 削除は別プロセス |

### Block: ひっかけ

表:

| 罠 | 正しい理解 |
|---|---|
| Primary Key = Subscriber Key | 違う |
| Email Address で個人を一意に識別できる | 不十分 |
| Publication List は除外リスト | 購読カテゴリ管理 |
| Suppression List は購読管理 | 送信除外 |
| Unsubscribed は All Subscribers から消える | ステータスとして残る |
| Data Filter で何でもできる | 複雑な JOIN は SQL |
| Sendable DE は EmailAddress だけあればよい | Subscriber Key との関係定義が必要 |
| Sendable DE からレコード削除 → Contact 消滅 | Contact 層は別。All Contacts には残る |
| All Contacts と All Subscribers は同じ | 違う。前者は全 BU 横断・全チャネル、後者は BU 内のメール購読者 |

---

## Page: §2.5 Insights and Analytics — page-meso-analytics

### Page Header
```html
<header class="page__header">
  <p class="page__subtitle">SECTION 2.5 — 配点 14%</p>
  <h1 class="page__title">Insights and Analytics</h1>
</header>
```

### Block: 何を問うか
> メール指標、パフォーマンス分析、レポート、Einstein 製品の選定。

### Block: 基本指標

表:

| 指標 | 意味 |
|---|---|
| Sent | 送信試行数 |
| Delivered | バウンスを除いた到達数 |
| Bounce | 配信失敗 |
| Hard Bounce | 恒久的失敗（無効アドレス等） |
| Soft Bounce | 一時的失敗（メールボックス満杯等） |
| Block Bounce | 受信側ポリシーによるブロック |
| Open / Unique Open / Total Open | 開封 / 一意開封 / 総開封 |
| Click / Unique Click / Total Click | クリック / 一意クリック / 総クリック |
| CTR (Click-Through Rate) | クリック数 / 配信数 |
| CTOR (Click-to-Open Rate) | クリック数 / 開封数 |
| Unsubscribe | 購読解除 |
| Complaint / Spam Complaint | 迷惑メール報告 |
| Conversion | 目的達成 |

### Block: 分析手段

表:

| 手段 | 用途 |
|---|---|
| Tracking (Email Studio) | 送信ジョブ単位の確認 |
| Standard Reports (Analytics Builder) | 標準レポート |
| Reports in Email Studio | 送信ジョブ別の詳細 |
| Data Views | SQL で `_Sent`、`_Open`、`_Click`、`_Bounce`、`_Subscribers`、`_Job`、`_ListSubscribers` 等を参照 |
| Tracking Extract | トラッキングデータをファイル化（過去最大 30 日） |
| Send Logging | 送信時点の属性を記録（要事前設定） |
| Journey Analytics | Journey 単位の分析 |
| Marketing Cloud Intelligence (旧 Datorama) | 高度な統合分析 |

### Block: Send Logging Data Extension の構造
リード:
> Send Log DE は事前に Salesforce サポートで有効化が必要。SendLog テンプレートから作成すると以下のフィールドが<em class="em">自動で値が入る</em>。

表:

| 自動収集フィールド | 内容 |
|---|---|
| JobID | 送信ジョブの一意識別子 |
| ListID | 送信に使用したリスト / DE の識別子 |
| BatchID | Triggered Send の同一 Job 内バッチ識別子 |
| SubID | Subscriber ID（Subscriber Key と別物・システム内部値） |
| TriggeredSendID | Triggered Send Definition の識別子 |
| ErrorCode | 送信失敗時のエラーコード |

カスタムフィールド追加ルール:
> Sendable DE のカラム名 / AMPscript 変数名 / Personalization String 名と<em class="em">完全に一致するカラム名</em>で DE に追加すれば、自動で値が入る。例：SubscriberKey、EmailAddress、FirstName、SubjectLine など。10 カラム以下推奨、すべて nullable 設定。

### Block: Data Views vs Tracking Extract vs Send Logging

表:

| 手段 | データ取得タイミング | 保持期間 | カスタマイズ |
|---|---|---|---|
| Data Views | リアルタイム参照 | 6 ヶ月（多くは） | 不可（標準カラム固定） |
| Tracking Extract | スケジュール抽出 | Tracking に依存 | Extract 条件のみ |
| Send Logging | 送信時点で記録 | DE 設定次第（永続可） | カスタムカラム追加可 |

### Block: Einstein 系の使い分け

表:

| 要件 | 製品・機能 |
|---|---|
| 開封・クリック・離脱可能性を予測 | Einstein Engagement Scoring |
| 受信者ごとの最適送信時刻 | Einstein Send Time Optimization (STO) |
| 送りすぎ / 少なすぎを判断 | Einstein Engagement Frequency |
| コンテンツを最適選択 | Einstein Content Selection |
| 件名を改善（生成 AI） | Einstein Copy Insights / Generative AI |
| 件名・本文の生成（多言語） | Einstein Generative AI for Email |
| キャンペーン効果を分析 | Einstein / Analytics 系機能 |

### Block: 最新論点（2026 年時点）

ul:
- <em class="em">VerifiedClicks</em>（SMS）: ボットやプレビューによる自動クリックを除外し、実際の購読者クリックに近い値を測定
- レポートデータ再処理: 7 日ウィンドウでサポートチケットなしに再処理可能
- 180 日保持: 送信・エンゲージメントデータは 180 日保持。長期分析は外部保存前提
- Data Extract キュー 250 件: 超過すると新規要求が拒否される。スケジュール過密に注意
- Journey Builder HTS: 大規模配信で利用可能な高速送信モード

### Block: ひっかけ

表:

| 罠 | 正しい理解 |
|---|---|
| Total と Unique を混同 | Unique は人単位、Total は総数 |
| CTR と CTOR を混同 | CTOR は開封後クリック |
| Open Rate を絶対視 | プライバシー保護 (Apple MPP<sup>*</sup>)・自動取得の影響を考慮 |
| 長期データは MC 内に無期限保持 | 180 日保持など制限を意識 |
| Extract は無制限にキューできる | キュー上限 250 件を意識 |
| Send Logging は何もせず使える | 事前に Salesforce サポートで有効化が必要 |
| Tracking Extract と Send Logging は同じ | Extract = 既存 Tracking から抽出、Logging = 送信時点で記録 |

## Term Tooltips to Add in Section 2

§2 で初出（または §0 / §1 で扱っていない）用語にツールチップ付与:

- **SPF**: Sender Policy Framework。送信元 IP を DNS で公開し、なりすまし送信を防ぐメール認証技術。
- **DKIM**: DomainKeys Identified Mail。メールに電子署名を付与し、改ざんと送信元を検証する技術。
- **DMARC**: Domain-based Message Authentication, Reporting and Conformance。SPF / DKIM の結果に基づき受信側の処理ポリシーを規定する。
- **AMPscript**: Marketing Cloud 独自のスクリプト言語。メール本文内で条件分岐・データ参照・差込を行う。
- **SSJS**: Server-Side JavaScript。Marketing Cloud のサーバー側で実行される JavaScript。AMPscript より柔軟。
- **RMM**: Reply Mail Management。返信メールの自動処理機能。Out-of-Office の除外、Bounce 処理、Unsubscribe 希望の処理を含む。
- **Apple MPP**: Apple Mail Privacy Protection。iOS 15 以降の Mail アプリでデフォルト有効。トラッキング画像を事前ロードするため Open Rate が実態より高く出る。
- **CTOR**: Click-to-Open Rate。クリック数 / 開封数。開封者のうち何 % がクリックしたか。

## Quality Checklist

- [ ] 6 ページすべて data-placeholder が削除されている
- [ ] page-meso にセクション索引カード 5 枚が表示される
- [ ] 索引カードクリックで対応するサブセクションに遷移する
- [ ] §2.1 〜 §2.5 各ページに上記コンテンツが入っている
- [ ] 各セクション h2 にコピーボタンがある
- [ ] 表、強調 (.em)、重要結論ブロック (.takeaway)、ASCII 図、用語ツールチップが既存パターンと一貫している
- [ ] 他のページ（page-top、page-prereq、page-macro）が引き続き正常動作する

## Out of Scope
- 他のページ（page-micro 以降）への変更
- §2 以外のコンテンツ追加
- 既存ページの変更