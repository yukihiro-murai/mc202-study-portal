# Section 7 (Part 2): 練習問題 Section 4-5 + 模試 — Implementation Spec

## Goal
index.html の練習問題ページ 3 つを実装する:
- page-quiz-section4: Section 4 Subscriber and Data Management (10 問)
- page-quiz-section5: Section 5 Insights and Analytics (10 問)
- page-quiz-mock: 模試 Mock Exam (20 問)

クイズの動作仕様は Part 1 で確立済みなので、本 Part ではコンテンツ流し込みのみ。

## Target File
index.html (単一ファイル維持)

## Scope of This Change
- §7 配下 3 ページのコンテンツ実装
- 新規 UI パターン追加なし（Part 1 で確立した .quiz と .quiz-nav を再利用）
- それ以外のページへの変更は不要

## Page Structure (同 Part 1)

各ページは以下の構造:
```html
<header class="page__header">
  <p class="page__subtitle">PRACTICE — SECTION 4</p>
  <h1 class="page__title">Subscriber and Data Management (10 問)</h1>
</header>

<nav class="quiz-nav" aria-label="問題索引">
  <p class="quiz-nav__label">問題索引</p>
  <a class="quiz-nav__item" href="#quiz-section4-q1">Q4-1</a>
  <!-- ... -->
</nav>

<article class="quiz" id="quiz-section4-q1" data-quiz-id="Q4-1" data-correct="C" data-type="single">
  <!-- ... -->
</article>
```

## Page: §7 Section 4 — page-quiz-section4

### Page Header
```html
<header class="page__header">
  <p class="page__subtitle">PRACTICE — SECTION 4</p>
  <h1 class="page__title">Subscriber and Data Management (10 問)</h1>
</header>
```

問題索引バー: Q4-1 〜 Q4-10

### Q4-1【単一選択】
- data-correct="C", data-type="single"
- 問題: 複数の属性、購入履歴、メール許諾を保持し、将来 SQL でセグメントを作成する予定。最適なデータ構造は？
- 選択肢:
  - A. List
  - B. Group
  - C. Data Extension
  - D. Publication List
- 根拠: 多属性・履歴・SQL 対応は Data Extension。
- 誤答理由: A, B は単純な購読者管理のみで属性拡張・SQL に弱い。D は購読カテゴリ管理機能。

### Q4-2【単一選択】
- data-correct="B", data-type="single"
- 問題: Data Extension 内の各行を一意識別するために使うのは？
- 選択肢:
  - A. Subscriber Key
  - B. Primary Key
  - C. Contact Key
  - D. Email Address
- 根拠: Primary Key は DE 内の行一意性。Subscriber Key は購読者識別子。
- 誤答理由: A は購読者識別子で DE 行とは別概念、C は Contact Builder 側の識別子、D は配信先で個人識別子としては不十分。

### Q4-3【単一選択】
- data-correct="B", data-type="single"
- 問題: Sendable DE で送信時に購読ステータスを正しく管理するために、必須の設定は？
- 選択肢:
  - A. Filtered DE への変換
  - B. Subscriber Relationship（DE のキーを Subscriber Key に紐付け）の設定
  - C. Send Logging の有効化
  - D. A/B Test Send の有効化
- 根拠: Sendable DE は Subscriber Relationship 設定で All Subscribers と連携。これがないと購読管理が機能しない。
- 誤答理由: A は条件抽出機能で本質ではない、C は送信記録、D はテスト機能。

### Q4-4【単一選択】
- data-correct="B", data-type="single"
- 問題: 家族で同じメールアドレスを共有する顧客がいる。各家族員に個別のメッセージを送りたい。最適な設計は？
- 選択肢:
  - A. Email Address を Subscriber Key にする
  - B. 各家族員に固有の Subscriber Key（顧客 ID）を割り当てる
  - C. すべて Suppression List に追加する
  - D. Publication List で分ける
- 根拠: Subscriber Key を個別に割り当てれば、同一メアドの中で別購読者として扱える。
- 誤答理由: A は家族員が同一購読者として扱われる、C は送信不可化、D はカテゴリ管理で個人識別とは別。

### Q4-5【複数選択 - 2つ選択】
- data-correct="A,C", data-type="multi-2"
- 問題: 送信予定数が 10,000 件のところ実送信が 8,500 件だった。最初に確認すべき項目は？（2 つ）
- 選択肢:
  - A. All Subscribers の購読ステータス（Unsubscribed / Held / Bounced）
  - B. Content Builder の画像サイズ
  - C. Suppression List / Exclusion List
  - D. Subscriber Preview の設定
- 根拠: 送信差異の主要原因は購読・除外レイヤー。
- 誤答理由: B はコンテンツの見た目で送信数とは無関係、D は事前確認機能。

### Q4-6【単一選択】
- data-correct="B", data-type="single"
- 問題: Publication List と Suppression List の違いとして正しいものは？
- 選択肢:
  - A. どちらも同じ機能
  - B. Publication List はカテゴリ別購読管理、Suppression List は送信除外
  - C. Publication List は除外用、Suppression List は購読管理
  - D. どちらも DE 内に作成する
- 根拠: Publication = カテゴリ別購読、Suppression = 送信除外。混同が頻出。
- 誤答理由: A は別機能、C は役割が逆、D は DE とは別の機能領域。

### Q4-7【単一選択】
- data-correct="B", data-type="single"
- 問題: Contact Key として最も推奨される値は？
- 選択肢:
  - A. Email Address
  - B. 不変の社内顧客 ID（CRM の顧客 ID など）
  - C. 電話番号
  - D. ユーザーの氏名
- 根拠: Email Address は変更・共有・複数所持が起こりうる。不変 ID が推奨される。
- 誤答理由: A は可変で不適、C も変更されうる、D は重複が多く識別不能。

### Q4-8【単一選択】
- data-correct="B", data-type="single"
- 問題: Sendable DE からレコードを削除した。All Contacts はどうなる？
- 選択肢:
  - A. 同時に削除される
  - B. Contact レコードは All Contacts に残る
  - C. Suppression List に自動追加される
  - D. Subscriber Key が無効化される
- 根拠: Contact Identity 層は Sendable DE とは独立。DE 削除では Contact は消えない。Contact 削除は別プロセス。
- 誤答理由: A は連動しない、C は手動操作、D は別概念。

### Q4-9【単一選択】
- data-correct="B", data-type="single"
- 問題: Parent BU と Child BU 3 つの構成で、共通テンプレートを複数の Child BU から参照したい。最適な手段は？
- 選択肢:
  - A. 各 Child BU に同じテンプレートを複製
  - B. Parent BU で Shared Data Extension / Shared Content を作成
  - C. Suppression List を Parent BU に集約
  - D. Send Classification を Parent BU に集約
- 根拠: Shared 系（DE / Content）は Parent で作成し子 BU から参照可能。複製はアンチパターン。
- 誤答理由: A はメンテ性悪化、C は購読除外で目的不一致、D は送信設定で共通テンプレートとは別。

### Q4-10【単一選択】
- data-correct="A", data-type="single"
- 問題: 特定の社内テスト用購読者については Open / Click を記録したくない。設定すべきは？
- 選択肢:
  - A. DoNotTrack Attribute を設定
  - B. Suppression List に追加
  - C. Sender Profile を変更
  - D. Send Logging を無効化
- 根拠: DoNotTrack Attribute は特定購読者のトラッキングを抑止する設定。
- 誤答理由: B は送信自体を止める、C は From 設定で関連なし、D は全体設定で個別制御不可。

## Page: §7 Section 5 — page-quiz-section5

### Page Header
```html
<header class="page__header">
  <p class="page__subtitle">PRACTICE — SECTION 5</p>
  <h1 class="page__title">Insights and Analytics (10 問)</h1>
</header>
```

問題索引バー: Q5-1 〜 Q5-10

### Q5-1【単一選択】
- data-correct="B", data-type="single"
- 問題: 開封者のうちクリックした割合を見たい。最も近い指標は？
- 選択肢:
  - A. CTR (Click-Through Rate)
  - B. CTOR (Click-to-Open Rate)
  - C. Bounce Rate
  - D. Complaint Rate
- 根拠: CTOR の分母は Open、CTR の分母は Delivered。
- 誤答理由: A は分母が Delivered で開封者ベースではない、C は到達失敗、D は苦情率。

### Q5-2【単一選択】
- data-correct="A", data-type="single"
- 問題: 受信者ごとの最適送信時刻を学習させたい。使う Einstein 機能は？
- 選択肢:
  - A. Einstein Send Time Optimization (STO)
  - B. Einstein Content Selection
  - C. Einstein Engagement Frequency
  - D. Einstein Copy Insights
- 根拠: STO は受信者個別最適時刻の機能。
- 誤答理由: B はコンテンツ選定、C は頻度判定、D は件名 / 本文改善。

### Q5-3【単一選択】
- data-correct="B", data-type="single"
- 問題: SMS リンクのクリック数にボットや URL プレビューによる自動クリックが混在している。実際の購読者クリックに近い値を見るには？
- 選択肢:
  - A. TotalClicks
  - B. VerifiedClicks
  - C. TotalOpens
  - D. BounceCount
- 根拠: VerifiedClicks（Spring '26）はボット由来の自動クリックを除外した補正指標。
- 誤答理由: A はボット込みの総数、C は開封指標、D はバウンス指標。

### Q5-4【単一選択】
- data-correct="A", data-type="single"
- 問題: 過去 30 日の送信履歴を SQL で参照したい。使う仕組みは？
- 選択肢:
  - A. Data Views（`_Sent`、`_Open` 等）
  - B. Send Classification
  - C. Subscriber Preview
  - D. Publication List
- 根拠: Data Views は Tracking 系データを SQL で参照する標準手段。
- 誤答理由: B は送信設定、C はプレビュー、D は購読管理で履歴参照不可。

### Q5-5【単一選択】
- data-correct="B", data-type="single"
- 問題: 送信時点の Subscriber 属性と Subject Line を永続保存したい。使う仕組みは？
- 選択肢:
  - A. Tracking Extract
  - B. Send Logging
  - C. Data Views
  - D. Suppression List
- 根拠: Send Logging は送信時点のスナップショットをカスタム DE に記録する。
- 誤答理由: A は既存 Tracking からの抽出で送信時点記録ではない、C は標準データで Subject Line 等は取れない、D は除外管理。

### Q5-6【複数選択 - 2つ選択】
- data-correct="A,C", data-type="multi-2"
- 問題: Send Logging Data Extension で自動的に値が入るフィールドは？（2 つ）
- 選択肢:
  - A. JobID
  - B. EmailContent
  - C. TriggeredSendID
  - D. CTR
- 根拠: 自動収集される 6 フィールドは JobID / ListID / BatchID / SubID / TriggeredSendID / ErrorCode。
- 誤答理由: B は本文で自動収集対象外、D は集計指標で送信時点では存在しない。

### Q5-7【単一選択】
- data-correct="A", data-type="single"
- 問題: Engagement の高い・低い購読者を予測したい。使う機能は？
- 選択肢:
  - A. Einstein Engagement Scoring
  - B. Einstein Send Time Optimization
  - C. Einstein Content Selection
  - D. Einstein Copy Insights
- 根拠: Engagement Scoring は開封・クリック・離脱可能性を予測する。
- 誤答理由: B は送信時刻、C はコンテンツ選定、D は文面改善。

### Q5-8【単一選択】
- data-correct="B", data-type="single"
- 問題: Apple Mail Privacy Protection (MPP) によって発生する現象は？
- 選択肢:
  - A. Click Rate が必ず低下する
  - B. Open Rate が実態より高く出る可能性がある
  - C. Bounce Rate が消失する
  - D. Send Classification が無効化される
- 根拠: MPP はトラッキング画像を事前ロードするため、実開封でなくても Open として計上される。
- 誤答理由: A はクリックには直接影響しない、C は別の指標、D は送信設定で MPP と無関係。

### Q5-9【単一選択】
- data-correct="C", data-type="single"
- 問題: Marketing Cloud の送信・エンゲージメントデータの保持期間は？（2026 年時点）
- 選択肢:
  - A. 30 日
  - B. 90 日
  - C. 180 日
  - D. 無期限
- 根拠: Spring '26 時点で 180 日。長期分析は外部退避が前提。
- 誤答理由: A, B は短すぎる、D は無期限保持ではない。

### Q5-10【単一選択】
- data-correct="B", data-type="single"
- 問題: Automation で Data Extract ジョブをスケジュールしているが、新規ジョブが拒否される事象が発生した。原因として最も可能性が高いのは？
- 選択肢:
  - A. Send Classification が未設定
  - B. Data Extract キューが上限 250 件を超えている
  - C. Publication List がない
  - D. Subscriber Preview が無効
- 根拠: Data Extract キュー上限は 250 件。スケジュール過密で詰まる典型パターン（Spring '26 で明文化）。
- 誤答理由: A は送信設定、C は購読管理、D はプレビュー機能で Extract キューと無関係。

## Page: §7 模試 Mock Exam — page-quiz-mock

### Page Header
```html
<header class="page__header">
  <p class="page__subtitle">PRACTICE — MOCK EXAM</p>
  <h1 class="page__title">模試 (20 問)</h1>
</header>
```

問題索引バー: M1 〜 M20

### Lead Paragraph
> 配点比例で構成された総合問題。Best Practices 2 問 / Content 5 問 / Automation 5 問 / Subscriber & Data 5 問 / Analytics 3 問。

### M1【単一選択】Best Practices
- data-correct="B", data-type="single"
- 問題: 新規顧客の獲得後に、4 週間にわたる Welcome シリーズ（5 通）を送信したい。各通のクリック有無で次の出し分けを変えたい。最適なツールは？
- 選択肢:
  - A. Automation Studio
  - B. Journey Builder
  - C. Triggered Send
  - D. User-Initiated Send
- 根拠: 個人時間軸 + 行動分岐は Journey Builder。
- 誤答理由: A はバッチ向きで個人時間軸不可、C は単発の即時送信、D は再利用可能定義の通常送信。

### M2【単一選択】Best Practices
- data-correct="A", data-type="single"
- 問題: 長期非開封の購読者を Suppression List に追加するか、再エンゲージメント Journey に投入するかを判断する基準として最も適切なのは？
- 選択肢:
  - A. 直近 30 日のエンゲージメント状況と過去の購入有無
  - B. Sender Profile の設定
  - C. Send Classification の Commercial / Transactional 分類
  - D. Subscriber Key の重複
- 根拠: 顧客価値とエンゲージメント履歴で判断するのが定石。
- 誤答理由: B は From 設定、C は送信分類、D は識別子問題で判断軸ではない。

### M3【単一選択】Content
- data-correct="C", data-type="single"
- 問題: メール内で会員ランクに応じた異なる商品レコメンドを表示したい。マーケターが GUI で管理できる手段は？
- 選択肢:
  - A. AMPscript の Lookup
  - B. SSJS
  - C. Dynamic Content
  - D. Personalization String
- 根拠: Dynamic Content は GUI で条件と表示内容を設定できる。
- 誤答理由: A, B は開発者向け、D は単純差込のみ。

### M4【複数選択 - 2つ選択】Content
- data-correct="A,C", data-type="multi-2"
- 問題: Sender Profile で管理される項目は？（2 つ）
- 選択肢:
  - A. From Name
  - B. Footer
  - C. Reply Mail Management (RMM)
  - D. Dedicated IP
- 根拠: From 系と Reply 処理は Sender Profile。Footer / IP は Delivery Profile。
- 誤答理由: B, D は Delivery Profile の管理項目。

### M5【単一選択】Content
- data-correct="B", data-type="single"
- 問題: Triggered Send Definition を作成後、Start していない状態で API から送信リクエストを送るとどうなる？
- 選択肢:
  - A. 自動的に Start される
  - B. 送信されない
  - C. Suppression List に追加される
  - D. Sender Profile が再生成される
- 根拠: Start していない Definition は送信を受け付けない。
- 誤答理由: A は明示的 Start が必須、C, D は無関係な動作。

### M6【単一選択】Content
- data-correct="B", data-type="single"
- 問題: A/B Test Send で Subject Line を比較する際、勝者基準（Winner Criteria）として最も適切なのは？
- 選択肢:
  - A. Bounce Rate
  - B. Open Rate（Subject Line のテストなので開封が直接の指標）
  - C. Send Classification
  - D. CTOR
- 根拠: Subject Line のテストは Open Rate が直接の効果指標。
- 誤答理由: A は到達性で件名効果ではない、C は送信設定、D は開封後の指標で件名効果の直接測定ではない。

### M7【単一選択】Content
- data-correct="A", data-type="single"
- 問題: Commercial 用と Transactional 用でメール送信設定を切り替えたい。再利用可能な設定単位は？
- 選択肢:
  - A. Send Classification
  - B. Sender Profile のみ
  - C. Delivery Profile のみ
  - D. Publication List
- 根拠: Send Classification = Sender + Delivery + CAN-SPAM 分類の組み合わせテンプレート。
- 誤答理由: B, C は単独では Commercial / Transactional 区分を持たない、D は購読カテゴリ。

### M8【単一選択】Automation
- data-correct="A", data-type="single"
- 問題: SFTP に毎朝 CSV が置かれる。これを DE に取り込み、対象者を絞ってメール送信したい。最適な Automation 構成は？
- 選択肢:
  - A. File Transfer → Import → SQL Query → Send Email
  - B. Send Email → File Transfer
  - C. Journey Builder の Wait → Engagement Split
  - D. Random Split → Dynamic Content
- 根拠: SFTP 取込・SQL 加工・送信のチェーンが Automation Studio の典型。
- 誤答理由: B は順序が逆、C は Journey Builder で取込不可、D は分岐とコンテンツ機能で取込関係なし。

### M9【単一選択】Automation
- data-correct="A", data-type="single"
- 問題: Data Extract を実行したが外部 SFTP に届かない。原因は？
- 選択肢:
  - A. File Transfer Activity が後続にない
  - B. Subscriber Key の重複
  - C. Send Classification の設定不備
  - D. Content Builder の画像エラー
- 根拠: Data Extract = ファイル化、File Transfer = 移動。両方必要。
- 誤答理由: B は DE 内部問題で SFTP と無関係、C は送信設定、D はメール本文。

### M10【単一選択】Automation
- data-correct="A", data-type="single"
- 問題: Welcome Journey が再登録時に再入場しない。設定変更すべきは？
- 選択肢:
  - A. Re-entry Settings
  - B. Goal
  - C. Exit Criteria
  - D. Decision Split
- 根拠: デフォルトは No re-entry。明示的な変更が必要。
- 誤答理由: B は目的達成定義、C は離脱条件、D は分岐機能。

### M11【複数選択 - 2つ選択】Automation
- data-correct="A,B", data-type="multi-2"
- 問題: Automation Studio で使う Activity として正しい組み合わせは？（2 つ）
- 選択肢:
  - A. SQL Query
  - B. File Transfer
  - C. Decision Split
  - D. Engagement Split
- 根拠: Decision / Engagement Split は Journey Builder の要素。
- 誤答理由: C, D は Journey Builder 内の要素で Automation Studio Activity ではない。

### M12【単一選択】Automation
- data-correct="B", data-type="single"
- 問題: Triggered Send が向く場面として最も適切なのは？
- 選択肢:
  - A. 定期的な一斉配信
  - B. EC 購入完了直後の購入確認メール
  - C. 月次レポートメール
  - D. Welcome 5 通シリーズ
- 根拠: イベント駆動・1 件単位・即時性は Triggered の典型。
- 誤答理由: A, C は定期バッチ向き、D は Journey Builder 向き。

### M13【単一選択】Subscriber & Data
- data-correct="A", data-type="single"
- 問題: Sendable DE と非 Sendable DE の違いは？
- 選択肢:
  - A. Sendable DE は Subscriber Relationship が設定されており送信対象として使える
  - B. 非 Sendable DE は Primary Key を持てない
  - C. Sendable DE は SQL で参照できない
  - D. 非 Sendable DE は Data Filter で抽出できない
- 根拠: Sendable DE の核心は Subscriber Relationship 設定の有無。
- 誤答理由: B, D は誤った技術的制約、C は SQL 参照は両方可能。

### M14【単一選択】Subscriber & Data
- data-correct="A", data-type="single"
- 問題: 実送信数が想定より少ない。最初に確認すべきは？
- 選択肢:
  - A. All Subscribers のステータス、Suppression / Exclusion
  - B. Subject Line
  - C. Send Time Optimization
  - D. Content Builder の Block 数
- 根拠: 送信差異の主要原因は購読・除外レイヤー。
- 誤答理由: B, D はコンテンツ要素、C は送信時刻最適化で差異とは別。

### M15【単一選択】Subscriber & Data
- data-correct="A", data-type="single"
- 問題: Email Address を Subscriber Key に使うリスクとして最も大きいのは？
- 選択肢:
  - A. メアド変更や家族共有で同一人物が複数 Subscriber 扱いになる
  - B. Send Classification が機能しなくなる
  - C. Sender Profile が無効化される
  - D. Dynamic Content が動かない
- 根拠: Email Address は変更・共有・複数所持のリスク。
- 誤答理由: B, C, D は Subscriber Key の選び方とは無関係。

### M16【単一選択】Subscriber & Data
- data-correct="B", data-type="single"
- 問題: 3 つの DE（顧客マスタ / 購入履歴 / Preference）を結合して対象者を抽出したい。最適な手段は？
- 選択肢:
  - A. Data Filter
  - B. SQL Query Activity
  - C. Subscriber Preview
  - D. Suppression Script
- 根拠: 複数 DE JOIN は SQL Query。
- 誤答理由: A は単一 DE 条件抽出、C は事前確認、D は除外スクリプト。

### M17【複数選択 - 2つ選択】Subscriber & Data
- data-correct="A,B", data-type="multi-2"
- 問題: Contact Key と Subscriber Key の関係として正しいものは？（2 つ）
- 選択肢:
  - A. ベストプラクティスは両者を同一値で運用する
  - B. Subscriber Key は Email Studio で使い、Contact Key は Contact Builder で使う
  - C. 両者は必ず別の値でなければならない
  - D. Contact Key は Email Address と同じである
- 根拠: 別レイヤーの識別子だが同一値運用が推奨。
- 誤答理由: C は同一値運用が推奨されている、D は Email Address は不変ではないため推奨されない。

### M18【単一選択】Analytics
- data-correct="B", data-type="single"
- 問題: 開封者の何 % がクリックしたかを見たい指標は？
- 選択肢:
  - A. CTR
  - B. CTOR
  - C. Bounce Rate
  - D. Complaint Rate
- 根拠: CTOR の分母は Open。
- 誤答理由: A は分母が Delivered、C, D は別指標。

### M19【単一選択】Analytics
- data-correct="A", data-type="single"
- 問題: 送信時点の SubscriberKey と Subject Line を後から SQL で参照できる形で残したい。最適な仕組みは？
- 選択肢:
  - A. Send Logging（カスタム DE にカラム名一致でフィールド追加）
  - B. Tracking Extract のみ
  - C. Send Classification
  - D. Data Filter
- 根拠: Send Logging は送信時点のスナップショットを永続保存。
- 誤答理由: B は標準カラムのみで Subject Line 等は取れない、C は送信設定、D は購読者抽出。

### M20【単一選択】Analytics
- data-correct="A", data-type="single"
- 問題: Marketing Cloud の送信・エンゲージメントデータが 180 日で消える。長期分析を行うためには？
- 選択肢:
  - A. Send Logging や Tracking Extract で外部退避し、別途分析環境で保持する
  - B. Send Classification を Commercial にする
  - C. Suppression List に追加する
  - D. Sender Profile を新規作成する
- 根拠: 保持期間制限のため、長期分析は外部退避前提。
- 誤答理由: B, C, D は保持期間とは無関係な機能。

## Quality Checklist

- [ ] page-quiz-section4, page-quiz-section5, page-quiz-mock の data-placeholder 属性を削除
- [ ] Section 4 ページに Q4-1 〜 Q4-10 (10 問) が表示
- [ ] Section 5 ページに Q5-1 〜 Q5-10 (10 問) が表示
- [ ] 模試ページに M1 〜 M20 (20 問) が表示
- [ ] 各ページに問題索引バー（Part 1 と同じ仕組み）
- [ ] 全問題で選択肢タップ → 即時判定 → 解説表示が動作
- [ ] 複数選択問題（4-5, 5-6, M4, M11, M17）で必要数選択時に自動判定
- [ ] 判定後ロック（再選択不可）
- [ ] リロードで初期状態に戻る
- [ ] 他のページが引き続き正常動作する

## Out of Scope
- スコア集計機能
- 進捗保存
- 問題のシャッフル / ランダム化