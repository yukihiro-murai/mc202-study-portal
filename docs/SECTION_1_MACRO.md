# Section 1: マクロ — Implementation Spec

## Goal
index.html の「1. マクロ」ページ（id="page-macro"）のプレースホルダーを削除し、
本コンテンツを実装する。
同時に、本章で新規に確立する「強調表現」「重要結論ブロック」「ASCIIフロー図」のスタイルを
他章でも使えるよう汎用的に実装する。

## Target File
index.html (単一ファイル維持)

## Scope of This Change
- 1. マクロページのコンテンツ実装
- 強調表現（emphasis）の共通スタイル追加（他章でも使う）
- 重要結論ブロックの共通スタイル追加（他章でも使う）
- ASCII フロー図の共通スタイル（§0 の BU ツリー図と統一）
- 他のページ（page-macro 以外）には変更を加えない

## Common UI Patterns to Establish

### 1. Inline Emphasis (軽い強調)
文中の単語やフレーズを目立たせる用途。

HTML 構造:
```html
<em class="em">境界判断</em>
```

CSS:
```css
.em {
  color: var(--accent);
  font-style: normal;
}
```

備考: <strong> ではなく <em class="em"> を使う（セマンティック的にも強調の意味）。
font-weight は変更しない（太字にしない）。

### 2. Key Takeaway Block (重要結論ブロック)
一段落級の結論、覚えてほしい1行を視覚的に独立させる。

HTML 構造:
```html
<div class="takeaway">
  Data + Automation で 52%、Content まで含めると 76%。どのセクションも「捨てる」判断は危険。
</div>
```

CSS:
```css
.takeaway {
  background: var(--accent-muted);
  border-left: 3px solid var(--accent);
  border-radius: var(--radius);
  padding: 14px 18px;
  margin: 16px 0;
  font-size: 1rem;
  line-height: 1.7;
  color: var(--text-primary);
}
```

備考: フォントサイズは本文より気持ち大きめ。

### 3. ASCII Flow Diagram
§0 で確立した <pre> スタイルをそのまま流用する（既に共通化されているはず）。
新規 CSS 追加不要。

## Section 1 Content to Implement

ページ構成（page-macro の中身）:

### Page Header
```html
<header class="page__header">
  <p class="page__subtitle">SECTION 1</p>
  <h1 class="page__title">マクロ：試験全体の構造・配点・攻略方針</h1>
</header>
```

### Section 1.1: 何を測る試験か

リード文:
> Email Specialist は単に「メールを作れるか」を見る試験ではなく、
> Marketing Cloud Engagement のメール領域で、<em class="em">顧客シナリオに対する設計判断ができるか</em>を問う試験。

具体的な観点リスト（h3「具体的には」+ ul）:
- どのデータ構造で購読者・属性・履歴を管理するか
- どの送信方式（Guided / User-Initiated / Triggered / Journey / Automation Send）を使うか
- どの自動化ツール（Automation Studio / Journey Builder / Triggered Send）を選ぶか
- 送信対象が想定より少ない/多い原因を切り分けられるか
- メール到達性・購読管理・法令対応を踏まえた設計ができるか
- トラッキング結果を正しく読み、必要なレポート手段を選べるか

### Section 1.2: 試験仕様（2026年現行）

表（data-table）:

| 項目 | 内容 |
|---|---|
| 問題数 | 60問（採点対象）+ 最大5問（非採点） |
| 試験時間 | 90分 |
| 合格点 | 67% |
| 形式 | 多肢選択・複数選択（multi-select）混在 |
| 受験料 | $200 USD / ¥30,000 JP（再受験 $100） |
| 受験形態 | テストセンター or オンライン Proctored |
| 持ち込み | 不可 |
| Mark for Review | 利用可（後で見直しできる） |

特筆すべき点（h3「特筆すべき点」+ ul）:
- 他の Salesforce 試験より <em class="em">複数選択（"Choose 2" / "Choose 3"）の比率が高い</em>。1問で2-3個正解させる形式に注意。
- 「best」「only」「all」「which two」「most appropriate」などの<em class="em">条件を限定する語</em>は決定的。
- 試験画面に下線・ハイライト機能はない。テストセンターでは<em class="em">スクラッチペーパー</em>、OnVUE では<em class="em">デジタルホワイトボード</em>が提供される。

### Section 1.3: 配点と優先順位

表（data-table）:

| 優先度 | セクション | 配点 | 戦略 |
|---|---|---|---|
| 1 | Subscriber and Data Management | 26% | 最優先。データ設計が崩れると他全部が崩れる |
| 2 | Marketing Automation | 26% | AS/JB/Triggered の使い分けが頻出 |
| 3 | Content Creation and Delivery | 24% | 送信設定・送信方式・コンテンツ管理が広い |
| 4 | Insights and Analytics | 14% | 軽視すると Einstein 系・Data Views で落とす |
| 5 | Email Marketing Best Practices | 10% | 配点低いが、到達性・法令の罠が多い |

重要結論ブロック（takeaway）:
> Data + Automation で 52%、Content まで含めると 76%。ただし複合シナリオが多く、どのセクションも「捨てる」判断は危険。

補足（h3「複合シナリオの例」+ ul）:
- Automation の問題に購読ステータスが絡む
- Content の問題に Send Classification が絡む
- Analytics の問題に Data Extract 制限が絡む

### Section 1.4: 出題思想の変遷

リード文:
> 旧バージョンでは Email Message Design や Tracking and Reporting が独立セクションだったが、
> 現行は<em class="em">より実務判断寄り</em>に再編されている。

ASCIIフロー図（pre タグ）:
```
メール作成
  ↓
データ構造を使ったパーソナライズ
  ↓
適切な送信方式
  ↓
Automation / Journey による運用
  ↓
Tracking / Analytics による改善
```

近年追加論点（h3「近年（Spring '25 / Spring '26 メンテナンスモジュール）の追加論点」+ ul）:
- Journey Builder High-Throughput Sending (HTS<sup>*</sup>)
- Einstein 生成 AI による多言語コンテンツ
- Automation Studio History Dashboard
- SMS の VerifiedClicks
- レポートデータ再処理（7日ウィンドウ）
- 送信・エンゲージメントデータ 180日保持
- Data Extract ジョブキュー上限 250件
- Path Optimizer (Journey Builder 内のA/Bテスト)

末尾の注意文:
> これらは試験コアを置き換えるものではないが、Analytics / Automation / Journey / Einstein の出題は軽くないことを示すシグナル。

### Section 1.5: 合格戦略の大枠

リード文:
> <em class="em">「機能名暗記」ではなく「境界判断」の試験</em>であることを意識する。

補足:
> 同じような機能名が並ぶが、問われるのは<em class="em">定義ではなく境界</em>。

表（data-table）「混同しやすい組み合わせと決め手」:

| 混同しやすい組み合わせ | 決め手 |
|---|---|
| Automation Studio vs Journey Builder | バッチ処理か、顧客体験か |
| SQL Query vs Data Filter | 複雑 JOIN か、単純条件か |
| Publication List vs Suppression List | 購読管理か、送信除外か |
| Sender Profile vs Delivery Profile | From か、配送方式か |
| Triggered Send vs User-Initiated Send | イベント即時か、再利用可能な通常送信か |
| Send Logging vs Tracking Extract | 送信時属性記録か、Tracking データ抽出か |
| All Contacts vs All Subscribers | Enterprise 横断か、BU 内の Email 購読者か |
| Contact Key vs Subscriber Key vs Primary Key | チャネル横断 ID vs Email Studio 識別子 vs DE 行一意性 |

## Term Tooltips to Add in Section 1

以下の用語は §1 で初出（または § 0 で扱っていない用語）。ツールチップ付与:

- **HTS**: High-Throughput Sending。Journey Builder で大規模配信時に利用できる高速送信モード。Spring '25 で正式化。
- **Mark for Review**: 試験中、後で見直したい問題に印を付ける機能。最後に見直し画面で確認できる。
- **OnVUE**: Pearson VUE が提供するオンラインプロクター（監督官）付き試験。自宅などから受験可能。
- **Multi-select**: 複数選択問題。"Choose 2" や "Choose 3" のように複数の正解を選ぶ形式。

## Quality Checklist
実装後、以下を確認:
- [ ] page-macro の data-placeholder 属性を削除
- [ ] page-macro に上記コンテンツが入っている
- [ ] 5 つのサブセクション（1.1〜1.5）が表示される
- [ ] 各セクション h2 の右にコピーボタンがある（§0 と同じ仕組み）
- [ ] 軽い強調（.em クラス）がアクセントカラーで表示される
- [ ] 重要結論ブロック（.takeaway クラス）が左アクセントボーダー付きで表示される
- [ ] ASCII フロー図が §0 と同じスタイルで表示される
- [ ] 用語ツールチップが §0 と同じ仕組みで動作する
- [ ] 他のページ（page-top、page-prereq）が引き続き正常動作する
- [ ] ダークトーンのデザイン一貫性が保たれている

## Out of Scope (This Change)
- 他のページ（page-meso 以降）への変更
- §1 以外のコンテンツ追加
- 既存ページの変更