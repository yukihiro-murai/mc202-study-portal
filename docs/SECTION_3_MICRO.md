# Section 3: ミクロ（判断シナリオ） — Implementation Spec

## Goal
index.html の「3. ミクロ」ページ (id="page-micro") のプレースホルダーを削除し、本コンテンツを実装する。
15 個の判断シナリオを「設問は常時表示、解答+判断軸+誤答理由はクリックで展開」の形式で実装する。

## Target File
index.html (単一ファイル維持)

## Scope of This Change
- page-micro のコンテンツ実装
- シナリオカード（.scenario）の共通スタイル追加（再利用想定）
- それ以外のページへの変更は不要

## Common UI Pattern to Establish

### Scenario Card
ミクロセクションで使用する判断シナリオの表示単位。

HTML 構造:
```html
<article class="scenario">
  <header class="scenario__head">
    <div class="scenario__number">3.1</div>
    <h3 class="scenario__title">購読者は 10,000 件、実送信は 8,700 件</h3>
  </header>
  <div class="scenario__question">
    <p class="scenario__label">設問</p>
    <p>Sendable DE には 10,000 件あるが、Send 成功は 8,700 件だった。最初に確認すべきものは？</p>
  </div>
  <button class="scenario__toggle" aria-expanded="false">
    <span class="scenario__toggle-label">解答と判断軸を見る</span>
    <span class="scenario__toggle-icon">▾</span>
  </button>
  <div class="scenario__answer" hidden>
    <div class="scenario__answer-row">
      <p class="scenario__label">最適解</p>
      <p class="scenario__solution">All Subscribers の購読ステータス（Unsubscribed / Held / Bounced）、Suppression List、Exclusion 設定</p>
    </div>
    <div class="scenario__answer-row">
      <p class="scenario__label">判断軸</p>
      <p>送信差異は購読・除外レイヤーで発生することが圧倒的に多い。データの中身を確認するのはその後。実務でも試験でも、購読層を上から下へ削っていく順序を最初に思い浮かべる癖をつける。</p>
    </div>
    <div class="scenario__answer-row scenario__answer-row--wrong" data-optional>
      <p class="scenario__label">誤答理由</p>
      <p>(該当する場合のみ記載)</p>
    </div>
  </div>
</article>
```

CSS:
```css
.scenario {
  background: var(--bg-surface);
  border: 1px solid var(--border-subtle);
  border-radius: var(--radius);
  padding: 20px 24px;
  margin-bottom: 16px;
}

.scenario__head {
  display: flex;
  align-items: baseline;
  gap: 12px;
  margin-bottom: 14px;
}

.scenario__number {
  flex-shrink: 0;
  color: var(--accent);
  font-size: 0.85rem;
  font-weight: 600;
  letter-spacing: 0.02em;
}

.scenario__title {
  margin: 0;
  font-size: 1.05rem;
  color: var(--text-primary);
}

.scenario__question {
  margin-bottom: 14px;
}

.scenario__label {
  font-size: 0.75rem;
  color: var(--text-tertiary);
  letter-spacing: 0.06em;
  text-transform: uppercase;
  margin: 0 0 4px;
}

.scenario__question p:not(.scenario__label) {
  margin: 0;
  color: var(--text-primary);
  line-height: 1.7;
}

.scenario__toggle {
  background: transparent;
  border: 1px solid var(--border-subtle);
  border-radius: var(--radius);
  color: var(--text-secondary);
  padding: 8px 14px;
  font-size: 0.85rem;
  display: inline-flex;
  align-items: center;
  gap: 8px;
  transition: border-color var(--transition), color var(--transition);
}

.scenario__toggle:hover {
  border-color: var(--accent);
  color: var(--accent);
}

.scenario__toggle-icon {
  font-size: 0.7rem;
  transition: transform var(--transition);
}

.scenario__toggle[aria-expanded="true"] .scenario__toggle-icon {
  transform: rotate(180deg);
}

.scenario__toggle[aria-expanded="true"] .scenario__toggle-label::before {
  content: "解答と判断軸を閉じる";
}

.scenario__toggle[aria-expanded="true"] .scenario__toggle-label {
  font-size: 0;
}

.scenario__toggle[aria-expanded="true"] .scenario__toggle-label::before {
  font-size: 0.85rem;
}

.scenario__answer {
  margin-top: 16px;
  padding-top: 16px;
  border-top: 1px solid var(--border-subtle);
}

.scenario__answer[hidden] {
  display: none;
}

.scenario__answer-row {
  margin-bottom: 12px;
}

.scenario__answer-row:last-child {
  margin-bottom: 0;
}

.scenario__solution {
  margin: 0;
  color: var(--accent);
  font-weight: 500;
  line-height: 1.7;
}

.scenario__answer-row p:not(.scenario__label):not(.scenario__solution) {
  margin: 0;
  color: var(--text-primary);
  line-height: 1.7;
}

.scenario__answer-row--wrong .scenario__label {
  color: var(--status-stale);
}
```

JS 要件:
- 各 .scenario__toggle のクリックで対応する .scenario__answer の hidden 属性をトグル
- aria-expanded 属性も同期して切替
- 個別動作（他のシナリオは閉じない）
- ボタンラベルは aria-expanded="true" のとき「解答と判断軸を閉じる」、false のとき「解答と判断軸を見る」

実装方法（簡潔版）:
```javascript
function bindScenarios() {
  document.querySelectorAll('.scenario__toggle').forEach(function (btn) {
    btn.addEventListener('click', function () {
      var expanded = btn.getAttribute('aria-expanded') === 'true';
      btn.setAttribute('aria-expanded', String(!expanded));
      var card = btn.closest('.scenario');
      var answer = card.querySelector('.scenario__answer');
      if (expanded) {
        answer.setAttribute('hidden', '');
      } else {
        answer.removeAttribute('hidden');
      }
      var label = btn.querySelector('.scenario__toggle-label');
      if (label) label.textContent = expanded ? '解答と判断軸を見る' : '解答と判断軸を閉じる';
    });
  });
}
```

(CSS で疑似要素を使った切替は複雑になるので、JS でラベル文言を直接切り替える方式を採用)

## Page: §3 ミクロ — page-micro

### Page Header
```html
<header class="page__header">
  <p class="page__subtitle">SECTION 3</p>
  <h1 class="page__title">ミクロ：判断シナリオと頻出ひっかけ</h1>
</header>
```

### Lead
> 複合シナリオで何を選ぶかの判断力を鍛えるパート。実試験では単一機能の定義を問う問題は少なく、複数の機能領域がまたがるシナリオで「最適 (most appropriate)」を選ばせる出題が多い。
> 
> 以下は試験形式に寄せたシナリオ集（本試験の問題ではなく、判断軸を学ぶための例）。

### Block: 各シナリオ（コピーボタンは block 単位で付ける必要なし、ページ全体は読むものとして扱う）

シナリオは 15 個。各 .scenario を縦に並べる。

#### 3.1 購読者は 10,000 件、実送信は 8,700 件
- 設問: Sendable DE には 10,000 件あるが、Send 成功は 8,700 件だった。最初に確認すべきものは？
- 最適解: All Subscribers の購読ステータス（Unsubscribed / Held / Bounced）、Suppression List、Exclusion 設定
- 判断軸: 送信差異は購読・除外レイヤーで発生することが圧倒的に多い。データの中身（メアドの妥当性、重複）を確認するのはその後。実務でも試験でも、購読層を上から下へ削っていく順序を最初に思い浮かべる癖をつける。
- 誤答理由: なし（省略）

#### 3.2 会員ランクに応じた異なるバナー表示
- 設問: 会員ランク（Gold / Silver / Bronze）によって異なるバナー画像を表示したい。マーケターが GUI で管理できる方法と、別 DE から条件参照する場合の方法は？
- 最適解: GUI 管理は <em class="em">Dynamic Content</em>、別 DE 参照を伴う条件分岐は <em class="em">AMPscript</em>（Lookup 関数で別 DE を参照）
- 判断軸: 「GUI 管理可能か / 開発者制御が必要か」「単純属性参照か / 別 DE 参照か」の 2 軸で切る。問題文中に「別の DE」「条件結合」が出てきたら AMPscript を疑うシグナル。
- 誤答理由: なし

#### 3.3 購入完了直後のメール
- 設問: EC サイトの購入完了時、外部システムから API で 1 通ずつ送信したい。
- 最適解: <em class="em">Triggered Send</em>
- 判断軸: 「外部イベント駆動」「1 件単位」「即時性」の 3 つが揃ったら Triggered。
- 誤答理由:<br>
  - Guided Send → 手動操作。API から呼び出せない<br>
  - Journey Builder → 顧客旅程に組み込むなら可能だが「1 通だけ即時送信」用途には過剰<br>
  - User-Initiated Send → 再利用可能な定義だが、1 件単位の API 呼び出しを想定していない

#### 3.4 Automation で Data Extract したのに SFTP に無い
- 設問: Automation Studio で Data Extract Activity は成功した。しかし外部 SFTP にファイルが見つからない。
- 最適解: Data Extract の後に <em class="em">File Transfer Activity</em> がない
- 判断軸: Data Extract = ファイル生成（Safehouse 内）、File Transfer = ファイル移動（Safehouse ⇄ SFTP）。この 2 つはセットで使うのが前提。試験でも実務でもここで詰まる人が非常に多い。
- 誤答理由: なし

(Safehouse 補足を term tooltip で：Safehouse: Marketing Cloud が提供する内部ファイル保管領域)

#### 3.5 フォーム登録 → Welcome → 3 日後にクリック有無で分岐
- 設問: 上記フローを実現する最適ツール
- 最適解: <em class="em">Journey Builder</em>
- 判断軸: 「個人ごとの時間軸 + 行動分岐」が出たら Journey。Automation Studio はバッチ処理向きで「個人ごとの 3 日後」を待つことができない（Wait で全体待機はできるが、個人時間軸ではない）。
- 誤答理由: なし

#### 3.6 ブランドごとに From Name とフッターを変える
- 設問: 同じメール本文だがブランド A とブランド B で From Name と Footer を変更したい。
- 最適解: <em class="em">Sender Profile</em>（From Name 用）と <em class="em">Delivery Profile</em>（Footer 用）をそれぞれブランド別に作成し、Send Classification として組み合わせる
- 判断軸: 「From」というキーワードに引きずられて Delivery Profile を選ぶと不正解。<br>
  - From Name / From Email → Sender Profile<br>
  - Reply 処理 (RMM) → Sender Profile<br>
  - Footer / Header / IP → Delivery Profile<br>
  - Commercial / Transactional → Send Classification の CAN-SPAM 分類
- 誤答理由: なし

#### 3.7 開封者のうちクリックした割合を見たい
- 設問: 開封したユーザーのうち、何 % がリンクをクリックしたか知りたい。
- 最適解: <em class="em">CTOR</em>（Click-to-Open Rate = Click / Open）
- 判断軸: CTR は分母が Delivered、CTOR は分母が Open。「開封してくれた人のうち」が出たら CTOR と即断できる癖をつける。
- 誤答理由: なし

#### 3.8 受信者ごとの最適時間に送りたい
- 設問: 受信者個別の反応傾向に合わせて送信時刻を最適化したい。
- 最適解: <em class="em">Einstein Send Time Optimization (STO)</em>
- 判断軸:<br>
  - 受信者ごと最適時刻 → STO<br>
  - 開封・クリック確率予測 → Engagement Scoring<br>
  - 送信頻度の過剰 / 不足判定 → Engagement Frequency<br>
  - コンテンツ自動選択 → Content Selection<br>
  - 件名・本文生成（多言語含む） → Copy Insights / Generative AI
- 誤答理由: なし

#### 3.9 SMS クリック数にボットが混在
- 設問: SMS キャンペーンのクリック数に、URL プレビューやセキュリティスキャナの自動クリックが混ざっている。実際の人間のクリックに近い値を見たい。
- 最適解: <em class="em">VerifiedClicks</em>（Spring '26 追加）
- 判断軸: Apple MPP 対応などプライバシー保護由来の自動クリック・自動開封は、メール / SMS 両方で測定値を歪める。Salesforce は VerifiedClicks で補正値を提供している。
- 誤答理由: なし

#### 3.10 BU 横断のキャンペーン管理
- 設問: Parent BU 1 つと Child BU 3 つの構成で、共通のテンプレートとロゴをすべての子 BU で使い回したい。
- 最適解: <em class="em">Shared Data Extension</em> / Shared Content（Parent BU で作成 → Child BU から参照）
- 判断軸: 「共通利用」「複数 BU」「親で一元管理」というキーワードは Shared 系のシグナル。各子 BU に同じものを複製するのはアンチパターン。
- 誤答理由: なし

#### 3.11 購読者がメアド変更で同一人物が 2 人にカウントされる
- 設問: 同じ顧客がメールアドレスを変えたら、Marketing Cloud 上で別 Contact として扱われた。原因は？
- 最適解: Contact Key として <em class="em">Email Address を使っている</em>こと
- 判断軸: Email Address は個人識別子として弱い。家族で共有、メアド変更、複数アドレス所持の可能性がある。Contact Key は不変の社内 ID（顧客 ID）を割り当てるのがベストプラクティス。試験でも頻出。
- 誤答理由: なし

#### 3.12 Send Logging を有効にしたい
- 設問: 送信時点の SubscriberKey / EmailAddress / SubjectLine を記録したい。
- 最適解:<br>
  1. Salesforce サポートに Send Logging 機能の有効化を依頼<br>
  2. SendLog テンプレートから DE を作成<br>
  3. DE のカラム名を、Sendable DE / AMPscript 変数 / Personalization String と<em class="em">完全に一致</em>させる
- 判断軸: Send Logging は事前申請が必要な機能。さらに、自動で値が入るのは 6 フィールド（JobID / ListID / BatchID / SubID / TriggeredSendID / ErrorCode）のみ。それ以外を取りたければカラム名の完全一致で自動マッピングさせる。
- 誤答理由: なし

#### 3.13 旧購読者を再エンゲージメントしたい
- 設問: 過去 6 ヶ月開封なしの購読者を再エンゲージメントするキャンペーンを設計したい。
- 最適解: <em class="em">Automation Studio で SQL Query</em> → 対象者 DE を生成 → <em class="em">Journey Builder</em> に投入
- 判断軸: 「過去 N 日間の○○」というデータ抽出は SQL Query が定番（Data View `_Open` を結合）。抽出後の顧客旅程は Journey Builder で分岐管理。両方を使う複合パターンは試験頻出。
- 誤答理由: なし

#### 3.14 Journey に再入場させたい
- 設問: Welcome Journey に 2 回目の登録時も入って欲しいが、デフォルト設定では入らない。
- 最適解: Journey の <em class="em">Re-entry Settings</em> を `Re-entry Anytime` または `Re-entry only after exiting` に変更
- 判断軸: デフォルトは `No re-entry`（一度入ったら終わり）。Welcome / Loyalty / 周年 Journey などで再入場を意図する場合は明示的に設定変更が必要。
- 誤答理由: なし

#### 3.15 A/B テスト勝者の自動配信
- 設問: 件名 A/B で 20% ずつ送り、CTOR 優秀な方を残り 60% に自動配信したい。
- 最適解: <em class="em">Email Studio の A/B Test Send</em> で件名 2 種・テスト割合各 20%・勝者基準 CTOR・勝者送信割合 60% を設定
- 判断軸: 違いを知る：<br>
  - <em class="em">A/B Test Send (Email Studio 内)</em>: 1 回の送信ジョブで件名や本文を比較<br>
  - <em class="em">Path Optimizer (Journey Builder 内)</em>: Journey 内の複数パスを比較し勝者パスへ寄せる<br>
  - <em class="em">Random Split (Journey Builder)</em>: 単純ランダム分岐（勝者選定機能なし）
- 誤答理由: なし

## Term Tooltips to Add in Section 3

§3 で初出の用語にツールチップ付与:

- **Safehouse**: Marketing Cloud が提供する内部ファイル保管領域。Automation Studio が処理するファイルは原則 Safehouse に置かれる。外部 SFTP とは File Transfer Activity で橋渡しする。
- **Path Optimizer**: Journey Builder 内で複数パスを A/B テストし、結果に基づき残りの購読者を勝者パスに送る機能。

(他の用語は §0〜§2 で既出のものを利用)

## Quality Checklist

- [ ] page-micro の data-placeholder 属性を削除
- [ ] 15 個のシナリオカードが縦に並んで表示される
- [ ] 各カードに「設問」が常時表示される
- [ ] 「解答と判断軸を見る」ボタンクリックで「最適解」「判断軸」「（あれば）誤答理由」が展開される
- [ ] 展開状態でボタンラベルが「解答と判断軸を閉じる」に変わる
- [ ] aria-expanded 属性が正しく切り替わる
- [ ] 個別動作（あるカードを開いても他のカードに影響しない）
- [ ] 強調 (.em) がアクセントカラーで表示される
- [ ] 用語ツールチップが既存パターンと一貫している
- [ ] 他のページが引き続き正常動作する

## Out of Scope
- 他のページへの変更
- §3 以外のコンテンツ追加
- 既存ページの変更