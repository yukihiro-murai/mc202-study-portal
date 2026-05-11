# Section 7 (Part 1): 練習問題 Section 1-3 + クイズ動作仕様 — Implementation Spec

## Goal
index.html の練習問題ページ 4 つを実装する:
- page-quiz: §7 全体の導入ページ（セクション索引）
- page-quiz-section1: Section 1 Email Marketing Best Practices (10 問)
- page-quiz-section2: Section 2 Content Creation and Delivery (10 問)
- page-quiz-section3: Section 3 Marketing Automation (10 問)

同時に、クイズの動作仕様（選択肢ロック、即時判定、解説表示、複数選択対応、問題間ナビ）を確立する。

## Target File
index.html (単一ファイル維持)

## Scope of This Change
- §7 配下 4 ページのコンテンツ実装
- クイズの共通 CSS / JS パターンを新規追加
- それ以外のページへの変更は不要

## Common UI Pattern to Establish

### 1. Quiz Question Block
1 問を表現するブロック。

HTML 構造:
```html
<article class="quiz" data-quiz-id="Q1-1" data-correct="B" data-type="single">
  <header class="quiz__head">
    <span class="quiz__id">Q1-1</span>
    <span class="quiz__type">単一選択</span>
  </header>
  <p class="quiz__question">問題文をここに記述。</p>
  <ul class="quiz__choices">
    <li>
      <button class="quiz__choice" data-choice="A">
        <span class="quiz__choice-mark">A</span>
        <span class="quiz__choice-text">選択肢A</span>
      </button>
    </li>
    <li>
      <button class="quiz__choice" data-choice="B">
        <span class="quiz__choice-mark">B</span>
        <span class="quiz__choice-text">選択肢B</span>
      </button>
    </li>
    <li>
      <button class="quiz__choice" data-choice="C">
        <span class="quiz__choice-mark">C</span>
        <span class="quiz__choice-text">選択肢C</span>
      </button>
    </li>
    <li>
      <button class="quiz__choice" data-choice="D">
        <span class="quiz__choice-mark">D</span>
        <span class="quiz__choice-text">選択肢D</span>
      </button>
    </li>
  </ul>
  <div class="quiz__explanation" hidden>
    <div class="quiz__verdict"></div>
    <div class="quiz__rationale">
      <p class="quiz__label">根拠</p>
      <p>解答の根拠説明</p>
    </div>
    <div class="quiz__wrong-reasons">
      <p class="quiz__label">誤答の理由</p>
      <p>他選択肢の不正解理由</p>
    </div>
  </div>
</article>
```

複数選択問題の場合は data-type="multi-2" や "multi-3"、data-correct は "A,C" のようにカンマ区切り。

### 2. CSS

```css
/* Quiz container */
.quiz {
  background: var(--bg-surface);
  border: 1px solid var(--border-subtle);
  border-radius: var(--radius);
  padding: 20px 24px;
  margin-bottom: 20px;
  scroll-margin-top: 24px;
}

.quiz__head {
  display: flex;
  align-items: baseline;
  gap: 12px;
  margin-bottom: 12px;
}

.quiz__id {
  color: var(--accent);
  font-weight: 600;
  font-size: 0.9rem;
  letter-spacing: 0.02em;
}

.quiz__type {
  color: var(--text-tertiary);
  font-size: 0.75rem;
  padding: 2px 8px;
  border: 1px solid var(--border-subtle);
  border-radius: var(--radius);
}

.quiz__question {
  color: var(--text-primary);
  line-height: 1.7;
  margin: 0 0 16px;
}

/* Choices */
.quiz__choices {
  list-style: none;
  margin: 0;
  padding: 0;
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.quiz__choice {
  width: 100%;
  text-align: left;
  background: transparent;
  border: 1px solid var(--border-default);
  border-radius: var(--radius);
  color: var(--text-primary);
  padding: 12px 16px;
  display: flex;
  align-items: flex-start;
  gap: 12px;
  font-size: 0.95rem;
  line-height: 1.6;
  transition: border-color var(--transition), background var(--transition);
  cursor: pointer;
}

.quiz__choice:hover:not(:disabled) {
  border-color: var(--accent);
  background: var(--bg-elevated);
}

.quiz__choice-mark {
  flex-shrink: 0;
  width: 24px;
  height: 24px;
  border-radius: 50%;
  background: var(--bg-elevated);
  color: var(--text-secondary);
  font-size: 0.8rem;
  font-weight: 600;
  display: inline-flex;
  align-items: center;
  justify-content: center;
}

.quiz__choice-text {
  flex: 1;
}

/* Selected states (after lock) */
.quiz__choice:disabled {
  cursor: default;
}

.quiz__choice.is-selected-correct {
  border-color: var(--accent);
  background: var(--accent-muted);
}

.quiz__choice.is-selected-correct .quiz__choice-mark {
  background: var(--accent);
  color: var(--bg-base);
}

.quiz__choice.is-selected-wrong {
  border-color: var(--status-stale);
  background: rgba(200, 113, 113, 0.08);
}

.quiz__choice.is-selected-wrong .quiz__choice-mark {
  background: var(--status-stale);
  color: var(--bg-base);
}

.quiz__choice.is-correct-answer {
  border-color: var(--accent);
}

.quiz__choice.is-correct-answer .quiz__choice-mark {
  border: 1px solid var(--accent);
  color: var(--accent);
  background: transparent;
}

/* Verdict and explanation */
.quiz__explanation {
  margin-top: 16px;
  padding-top: 16px;
  border-top: 1px solid var(--border-subtle);
}

.quiz__explanation[hidden] {
  display: none;
}

.quiz__verdict {
  font-size: 0.95rem;
  font-weight: 600;
  margin-bottom: 12px;
}

.quiz__verdict.is-correct {
  color: var(--accent);
}

.quiz__verdict.is-wrong {
  color: var(--status-stale);
}

.quiz__label {
  font-size: 0.75rem;
  color: var(--text-tertiary);
  letter-spacing: 0.06em;
  text-transform: uppercase;
  margin: 0 0 4px;
}

.quiz__rationale,
.quiz__wrong-reasons {
  margin-bottom: 12px;
}

.quiz__rationale:last-child,
.quiz__wrong-reasons:last-child {
  margin-bottom: 0;
}

.quiz__rationale p:not(.quiz__label),
.quiz__wrong-reasons p:not(.quiz__label) {
  margin: 0;
  color: var(--text-primary);
  line-height: 1.7;
}

/* Question index nav bar */
.quiz-nav {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
  margin-bottom: 24px;
  padding: 12px 16px;
  background: var(--bg-surface);
  border: 1px solid var(--border-subtle);
  border-radius: var(--radius);
}

.quiz-nav__label {
  font-size: 0.75rem;
  color: var(--text-tertiary);
  letter-spacing: 0.06em;
  text-transform: uppercase;
  width: 100%;
  margin: 0 0 8px;
}

.quiz-nav__item {
  display: inline-block;
  padding: 4px 10px;
  border: 1px solid var(--border-subtle);
  border-radius: var(--radius);
  font-size: 0.8rem;
  color: var(--text-secondary);
  text-decoration: none;
  transition: border-color var(--transition), color var(--transition);
}

.quiz-nav__item:hover {
  border-color: var(--accent);
  color: var(--accent);
}
```

### 3. JavaScript

```javascript
function bindQuizzes() {
  document.querySelectorAll('.quiz').forEach(function (quiz) {
    var correct = (quiz.getAttribute('data-correct') || '').split(',').map(function (s) { return s.trim(); });
    var type = quiz.getAttribute('data-type') || 'single';
    var requiredCount = type === 'single' ? 1 : parseInt(type.split('-')[1], 10) || 1;
    var selectedChoices = [];
    var locked = false;

    quiz.querySelectorAll('.quiz__choice').forEach(function (btn) {
      btn.addEventListener('click', function () {
        if (locked) return;
        var choice = btn.getAttribute('data-choice');

        // Toggle selection (only for multi-select)
        if (type !== 'single') {
          var idx = selectedChoices.indexOf(choice);
          if (idx === -1) {
            if (selectedChoices.length >= requiredCount) return; // can't select more
            selectedChoices.push(choice);
            btn.classList.add('is-pending');
          } else {
            selectedChoices.splice(idx, 1);
            btn.classList.remove('is-pending');
          }
        } else {
          selectedChoices = [choice];
        }

        // Check if ready to evaluate
        if (selectedChoices.length === requiredCount) {
          evaluateQuiz(quiz, correct, selectedChoices);
          locked = true;
        }
      });
    });
  });
}

function evaluateQuiz(quiz, correct, selected) {
  // Sort for comparison
  var correctSorted = correct.slice().sort().join(',');
  var selectedSorted = selected.slice().sort().join(',');
  var isCorrect = correctSorted === selectedSorted;

  // Apply visual states
  quiz.querySelectorAll('.quiz__choice').forEach(function (btn) {
    var choice = btn.getAttribute('data-choice');
    btn.disabled = true;
    btn.classList.remove('is-pending');

    if (selected.indexOf(choice) !== -1) {
      // User selected this
      if (correct.indexOf(choice) !== -1) {
        btn.classList.add('is-selected-correct');
      } else {
        btn.classList.add('is-selected-wrong');
      }
    } else if (correct.indexOf(choice) !== -1) {
      // Correct answer that user didn't select
      btn.classList.add('is-correct-answer');
    }
  });

  // Show explanation
  var explanation = quiz.querySelector('.quiz__explanation');
  if (explanation) {
    explanation.removeAttribute('hidden');
    var verdict = explanation.querySelector('.quiz__verdict');
    if (verdict) {
      verdict.textContent = isCorrect ? '◯ 正解' : '× 不正解';
      verdict.classList.add(isCorrect ? 'is-correct' : 'is-wrong');
    }
  }
}
```

`.quiz__choice.is-pending` のスタイル（複数選択中の暫定選択状態）:
```css
.quiz__choice.is-pending {
  border-color: var(--accent);
  background: var(--accent-muted);
}
```

### 4. Page Structure for Each Quiz Section

各セクションページの構造:
```html
<header class="page__header">
  <p class="page__subtitle">PRACTICE — SECTION 1</p>
  <h1 class="page__title">Email Marketing Best Practices (10 問)</h1>
</header>

<nav class="quiz-nav" aria-label="問題索引">
  <p class="quiz-nav__label">問題索引</p>
  <a class="quiz-nav__item" href="#quiz-section1-q1">Q1-1</a>
  <a class="quiz-nav__item" href="#quiz-section1-q2">Q1-2</a>
  <!-- ... Q1-10 -->
</nav>

<article class="quiz" id="quiz-section1-q1" data-quiz-id="Q1-1" data-correct="B" data-type="single">
  <!-- ... -->
</article>
```

問題索引のクリックで該当問題までスクロール（CSS `scroll-margin-top` で位置調整済み）。

## Page: §7 練習問題 導入 — page-quiz

### Page Header
```html
<header class="page__header">
  <p class="page__subtitle">SECTION 7</p>
  <h1 class="page__title">練習問題 70 問</h1>
</header>
```

### Lead
> 配点比例で構成された練習問題集。Section 1-5 各 10 問 + 模試 20 問。
> 各問題は選択肢タップで即座に正解 / 不正解と解説を表示する。
> 一度選択すると変更不可（リロードで初期化される）。

### Section Index Cards (§2 メソと同じパターンを再利用)
6 枚のカード:
- Section 1 / Email Marketing Best Practices / 10 問
- Section 2 / Content Creation and Delivery / 10 問
- Section 3 / Marketing Automation / 10 問
- Section 4 / Subscriber and Data Management / 10 問
- Section 5 / Insights and Analytics / 10 問
- 模試 / 配点比例の総合問題 / 20 問

各カードクリックで対応セクションへ遷移。

## Page: §7 Section 1 — page-quiz-section1

10 問。問題索引バー + 10 個の .quiz ブロック。

### Q1-1【単一選択】
- data-correct="B", data-type="single"
- 問題: ある企業が、これまで送信実績のない新しい専用 IP から 10 万件の一斉配信を開始したい。最も推奨される行動は？
- 選択肢:
  - A. すぐに 10 万件を 1 回で送信する
  - B. IP warming を段階的に実施し、徐々に送信量を増やす
  - C. Sender Profile を新規作成して即時送信する
  - D. Tracking Extract で事前に到達率を測定する
- 根拠: 新規 IP は送信者評価（reputation）がゼロ。いきなり大量送信すると ISP からスパム判定される。IP warming で少量から段階的に送信量を増やし評価を確立するのが定石。
- 誤答理由: A は確実にスパム判定。C は Sender Profile を新規作成しても warming は別問題。D は事前測定は warming の代替にならない。

### Q1-2【複数選択 - 2つ選択】
- data-correct="B,C", data-type="multi-2"
- 問題: メール到達性に直接影響する要素は？（2 つ選択）
- 選択肢:
  - A. メール本文のフォントサイズ
  - B. SPF / DKIM / DMARC の認証設定
  - C. Sender reputation
  - D. Content Builder の Block の数
  - E. Email Studio の Tracking タブの設定
- 根拠: 認証設定（SPF/DKIM/DMARC）と送信者評価は到達性の中核要因。
- 誤答理由: A, D はコンテンツの見た目に関わるが到達性そのものには直接影響しない。E は事後の集計設定。

### Q1-3【単一選択】
- data-correct="B", data-type="single"
- 問題: CAN-SPAM 法に準拠するために、Commercial Email に必須の要素は？
- 選択肢:
  - A. ピクセル画像による Open Tracking
  - B. 物理住所の明記と Unsubscribe リンク
  - C. A/B Testing の実施
  - D. Dynamic Content による出し分け
- 根拠: CAN-SPAM は送信者の物理住所明記と容易な購読解除手段の提供を求める。
- 誤答理由: A, C, D はマーケティング上の手段であり法令要件ではない。

### Q1-4【単一選択】
- data-correct="B", data-type="single"
- 問題: Hard Bounce と Soft Bounce の違いとして正しいものは？
- 選択肢:
  - A. Hard Bounce は一時的な失敗、Soft Bounce は恒久的な失敗
  - B. Hard Bounce は恒久的な失敗、Soft Bounce は一時的な失敗
  - C. どちらも同じ意味
  - D. Hard Bounce は購読者側、Soft Bounce は ISP 側の問題
- 根拠: Hard = アドレス無効・存在しないなど恒久的。Soft = 一時的（メールボックス満杯、サーバー一時障害）。
- 誤答理由: A は定義の逆。C は明確に区別される概念。D は分類軸が異なる。

### Q1-5【単一選択】
- data-correct="B", data-type="single"
- 問題: 購読者獲得時に CAN-SPAM / GDPR の観点で最も望ましいのは？
- 選択肢:
  - A. 既存購入者リストにオプトインなしで一斉送信
  - B. Web サイトのフォームで明示的なオプトインを取得し、Welcome メールで再確認
  - C. 購入後に自動で全マーケティングメールを送信
  - D. 競合の購読者リストを購入
- 根拠: 明示的同意 + Double Opt-in（再確認）は法令・到達性の両面で最も安全。
- 誤答理由: A, C は同意取得が不十分。D は明確な法令違反。

### Q1-6【単一選択】
- data-correct="B", data-type="single"
- 問題: メールの開封率が低下している場合に、まず確認すべき要素として最も適切なものは？
- 選択肢:
  - A. メール本文の文字数
  - B. Subject Line / Preheader と送信者評価・リスト品質
  - C. Content Builder のブロック数
  - D. Send Logging の有効化状況
- 根拠: 開封の主要ドライバは Subject Line、Preheader、From、送信者評価、リスト品質。
- 誤答理由: A は開封後の指標、C は構造要素、D は送信記録設定で開封率の主要因ではない。

### Q1-7【複数選択 - 2つ選択】
- data-correct="A,C", data-type="multi-2"
- 問題: Dedicated IP を導入するメリットとして適切なものは？（2 つ）
- 選択肢:
  - A. 送信者評価を自社のみでコントロールできる
  - B. 即座にすべての送信が成功する
  - C. 大量送信時の到達性管理がしやすい
  - D. すべての企業に必須である
- 根拠: Dedicated IP はレピュテーション独立管理と大量配信向き。
- 誤答理由: B は warming が必要。D は中小送信量では Shared IP の方が安定する場合もあり、必須ではない。

### Q1-8【単一選択】
- data-correct="B", data-type="single"
- 問題: モバイル対応のメールデザインで最も重要な原則は？
- 選択肢:
  - A. すべての画像を最大サイズで埋め込む
  - B. レスポンシブデザインとシングルカラム構造で、画像非表示でも内容が伝わる
  - C. テキストを最小化し画像中心にする
  - D. ボタンサイズを最小にする
- 根拠: モバイルでは画像非表示やナローカラム表示に耐える設計が必須。
- 誤答理由: A は読み込み遅延の原因。C はアクセシビリティと到達性に悪影響。D はタップしにくくなる。

### Q1-9【単一選択】
- data-correct="B", data-type="single"
- 問題: リストハイジーン（list hygiene）を維持するための最も適切な施策は？
- 選択肢:
  - A. すべての購読者を毎月削除する
  - B. バウンス・長期非エンゲージメント購読者を定期的に除外 / 再エンゲージメント施策に分離する
  - C. 同一購読者を複数回登録して送信機会を増やす
  - D. Email Address を Subscriber Key として使う
- 根拠: Bounce や長期非反応者を残すと送信者評価が悪化。再エンゲージメント Journey や除外設定が定石。
- 誤答理由: A はリスト破壊。C は重複と苦情の原因。D は識別子の問題で hygiene には直結しない。

### Q1-10【単一選択】
- data-correct="A", data-type="single"
- 問題: Preference Center を提供する目的として最も適切なものは？
- 選択肢:
  - A. 購読者に購読カテゴリやメール頻度を選択させて、購読維持率を上げる
  - B. Suppression List を自動で削除する
  - C. Send Classification を購読者に編集させる
  - D. Sender Profile を購読者ごとに変更する
- 根拠: Preference Center は購読カテゴリ・頻度を購読者自身に管理させ、強制的な unsubscribe を回避するための仕組み。
- 誤答理由: B は管理側機能で購読者は触れない。C, D は送信者側の設定で購読者には公開しない。

## Page: §7 Section 2 — page-quiz-section2

10 問。

### Q2-1【単一選択】
- data-correct="C", data-type="single"
- 問題: 会員ランク（Gold / Silver / Bronze）によって異なるバナー画像を表示したい。マーケターが GUI で管理できる方法は？
- 選択肢:
  - A. AMPscript の IIF 関数
  - B. SSJS によるサーバー処理
  - C. Dynamic Content
  - D. Personalization String のみ
- 根拠: Dynamic Content は GUI で条件と表示内容を設定できる。マーケターによる運用に向く。
- 誤答理由: A, B は開発者向けでマーケター単独運用には不向き。D は単純差込のみで条件分岐不可。

### Q2-2【単一選択】
- data-correct="B", data-type="single"
- 問題: 別の Data Extension から顧客の最新購入商品を引いてきて、メール本文に表示したい。最も適切な手段は？
- 選択肢:
  - A. Personalization String %%LastProduct%%
  - B. AMPscript の Lookup 関数
  - C. Dynamic Content
  - D. Send Classification
- 根拠: 別 DE 参照には Lookup("DE名", "返却カラム", "検索カラム", "検索値") が定番。
- 誤答理由: A は Sendable DE 内のカラム差込のみ。C は GUI 条件分岐で別 DE 参照は不可。D は送信設定であり機能が異なる。

### Q2-3【単一選択】
- data-correct="B", data-type="single"
- 問題: 新ブランドの立ち上げで、From Name を「Brand X」、From Email を「news@brandx.com」に変更したい。設定する場所は？
- 選択肢:
  - A. Delivery Profile
  - B. Sender Profile
  - C. Send Classification の CAN-SPAM 分類
  - D. Publication List
- 根拠: From Name / From Email は Sender Profile の管理項目。
- 誤答理由: A は配送設定（IP / Footer / Header）、C は商用 / 取引区分、D は購読カテゴリ管理。

### Q2-4【単一選択】
- data-correct="B", data-type="single"
- 問題: 特定ブランドのメールで、フッターと配送 IP を変更したい。設定する場所は？
- 選択肢:
  - A. Sender Profile
  - B. Delivery Profile
  - C. Subscriber Preview
  - D. Content Block
- 根拠: Footer / Header / IP は Delivery Profile の管理項目。
- 誤答理由: A は From 系設定、C はプレビュー機能、D はコンテンツ部品でグローバル配送設定には使えない。

### Q2-5【単一選択】
- data-correct="A", data-type="single"
- 問題: Commercial と Transactional のメールを区別し、Sender Profile と Delivery Profile を組み合わせて再利用可能な送信テンプレートを作成したい。使うべきものは？
- 選択肢:
  - A. Send Classification
  - B. Publication List
  - C. Subscriber Filter
  - D. Content Block
- 根拠: Send Classification = Sender Profile + Delivery Profile + CAN-SPAM 分類。
- 誤答理由: B は購読カテゴリ管理、C は購読者抽出、D はコンテンツ部品。

### Q2-6【複数選択 - 2つ選択】
- data-correct="A,C", data-type="multi-2"
- 問題: A/B Test Send で勝者基準（Winner Criteria）として選択可能な指標は？（2 つ）
- 選択肢:
  - A. Click-Through Rate
  - B. Bounce Rate
  - C. Open Rate
  - D. Send Classification
- 根拠: A/B Test の勝者判定は Open / Click / CTOR などの反応指標。
- 誤答理由: B は到達性指標で勝者判定には不適。D は送信設定で指標ではない。

### Q2-7【単一選択】
- data-correct="C", data-type="single"
- 問題: EC サイトの購入完了直後に、外部システムから API で 1 通の購入完了メールを即時送信したい。最も適切な送信方式は？
- 選択肢:
  - A. Guided Send
  - B. User-Initiated Send
  - C. Triggered Send
  - D. Automation Studio Send Email Activity
- 根拠: API 駆動・即時・1 件単位は Triggered Send の典型用途。
- 誤答理由: A は手動操作、B は再利用可能定義の通常送信、D はバッチ処理。

### Q2-8【単一選択】
- data-correct="B", data-type="single"
- 問題: メール送信前に、特定の購読者属性での見え方を確認したい。最も適切な手段は？
- 選択肢:
  - A. Test Send で自分だけに送信
  - B. Subscriber Preview で実購読者データを使って表示確認
  - C. Tracking Extract
  - D. Data Filter
- 根拠: Subscriber Preview は実購読者データを使った事前確認機能。
- 誤答理由: A は自分の属性での確認のみ、C は事後分析、D は購読者抽出機能。

### Q2-9【単一選択】
- data-correct="A", data-type="single"
- 問題: メール送信前に承認ワークフロー（複数人レビュー → 承認 → 送信）を組み込みたい。使うべき機能は？
- 選択肢:
  - A. Approvals
  - B. Send Logging
  - C. Publication List
  - D. Triggered Send
- 根拠: Approvals は Marketing Cloud の事前承認ワークフロー機能。
- 誤答理由: B は送信記録、C は購読管理、D は API 駆動送信で承認とは無関係。

### Q2-10【複数選択 - 2つ選択】
- data-correct="A,B", data-type="multi-2"
- 問題: AMPscript と Dynamic Content の使い分けとして正しいものは？（2 つ）
- 選択肢:
  - A. 開発者が複雑なロジックを書く場合は AMPscript
  - B. マーケターが GUI で管理する場合は Dynamic Content
  - C. AMPscript は Personalization String と完全に同じ機能
  - D. Dynamic Content は別 DE を参照できない場合がある
- 根拠: AMPscript = 開発者制御、Dynamic Content = GUI 管理。
- 誤答理由: C は AMPscript が条件分岐 / 関数 / 別 DE 参照など Personalization String 以上の機能を持つため誤り。D は技術的には正だが、本問では A・B が代表的な使い分け軸として優先される。

## Page: §7 Section 3 — page-quiz-section3

10 問。

### Q3-1【単一選択】
- data-correct="B", data-type="single"
- 問題: 毎朝 SFTP に置かれた CSV を取り込み、SQL で対象者を絞り、対象 DE を更新したい。最適な構成は？
- 選択肢:
  - A. Journey Builder の Decision Split のみ
  - B. File Transfer → Import → SQL Query
  - C. A/B Test Send → Send Classification
  - D. Subscriber Preview → Test Send
- 根拠: ファイル取込・SQL 加工は Automation Studio の Activity を順次実行する典型パターン。
- 誤答理由: A は分岐機能のみで取込不可、C は送信設定、D は事前確認機能。

### Q3-2【単一選択】
- data-correct="A", data-type="single"
- 問題: Automation で Data Extract Activity は成功したが、外部 SFTP にファイルが届かない。最も可能性が高い原因は？
- 選択肢:
  - A. Data Extract の後に File Transfer Activity が無い
  - B. Send Classification が Commercial でない
  - C. Subscriber Key が重複している
  - D. Dynamic Content が無効
- 根拠: Data Extract はファイル生成（Safehouse 内）、File Transfer は移動。両方必要。
- 誤答理由: B はメール送信設定で Extract と無関係、C は DE 内の問題で SFTP 転送と無関係、D はメール本文機能。

### Q3-3【単一選択】
- data-correct="B", data-type="single"
- 問題: フォーム登録直後に Welcome、3 日後にクリック有無で分岐したい。最適なツールは？
- 選択肢:
  - A. Automation Studio
  - B. Journey Builder
  - C. Tracking Extract
  - D. Publication List
- 根拠: 個人ごとの時間軸 + 行動分岐は Journey Builder。
- 誤答理由: A はバッチ向きで個人時間軸の待機不可、C は分析機能、D は購読管理。

### Q3-4【単一選択】
- data-correct="B", data-type="single"
- 問題: 3 つの DE（顧客マスタ / 購入履歴 / Preference）を JOIN して対象者 DE を毎週更新したい。使うべき Activity は？
- 選択肢:
  - A. Data Filter
  - B. SQL Query Activity
  - C. Send Email Activity
  - D. Verification Activity
- 根拠: 複数 DE JOIN は SQL Query。Data Filter は単純条件のみ。
- 誤答理由: A は単一 DE 内の条件抽出、C はメール送信、D は件数検証。

### Q3-5【単一選択】
- data-correct="A", data-type="single"
- 問題: Journey で Welcome 後 7 日経過した購読者にのみ次のメールを送りたい。使う要素は？
- 選択肢:
  - A. Wait Activity（7 日）
  - B. Random Split
  - C. Data Filter Activity
  - D. File Transfer
- 根拠: 個人タイムライン上の待機は Wait Activity。
- 誤答理由: B はランダム分岐、C は Automation Studio の機能、D はファイル移動。

### Q3-6【複数選択 - 2つ選択】
- data-correct="A,C", data-type="multi-2"
- 問題: Triggered Send Definition のライフサイクルとして正しいものは？（2 つ）
- 選択肢:
  - A. Create → Verify → Start
  - B. Create → Start のみで運用可能
  - C. Definition を Start しないと API 経由でも送信されない
  - D. Send Classification があれば Definition は不要
- 根拠: Triggered Send は Definition を作成 → 検証 → Start で初めて稼働。Start しないと外部からの送信リクエストを受けない。
- 誤答理由: B は Verify 工程が必須なので誤り、D は Definition と Send Classification は別の概念。

### Q3-7【単一選択】
- data-correct="B", data-type="single"
- 問題: 過去 6 ヶ月開封のない購読者を抽出し、再エンゲージメント Journey に投入したい。最適な組み合わせは？
- 選択肢:
  - A. Journey Builder の Decision Split のみで完結
  - B. Automation Studio の SQL Query で抽出 DE を作成し、Journey の Data Extension Entry に設定
  - C. Tracking Extract で抽出し手動でアップロード
  - D. Suppression List に追加する
- 根拠: 過去 N 日間データの抽出は SQL Query、Journey への投入は DE Entry が定石。
- 誤答理由: A は事前抽出機能なし、C は手動運用で自動化されない、D は再送ではなく除外。

### Q3-8【単一選択】
- data-correct="A", data-type="single"
- 問題: Welcome Journey に同じ購読者が 2 回目の登録で再度入って欲しいが、デフォルトでは入らない。設定すべき項目は？
- 選択肢:
  - A. Re-entry Settings を「Re-entry Anytime」または「Re-entry only after exiting」に変更
  - B. Exit Criteria を削除
  - C. Goal を変更
  - D. Path Optimizer を有効化
- 根拠: デフォルトは No re-entry。明示的な変更が必要。
- 誤答理由: B は離脱条件で再入場制御ではない、C は目的達成定義、D は A/B テスト機能。

### Q3-9【単一選択】
- data-correct="B", data-type="single"
- 問題: Journey の Path Optimizer と Email Studio の A/B Test Send の違いとして正しいものは？
- 選択肢:
  - A. どちらも同じ機能で名前だけ違う
  - B. Path Optimizer は Journey 内の複数パスを比較、A/B Test Send は 1 回の送信内で件名等を比較
  - C. A/B Test Send は Journey 内でのみ使用可能
  - D. Path Optimizer は Automation Studio で動作する
- 根拠: 両者は実行レイヤーが異なる。
- 誤答理由: A は同一視は誤り、C は Email Studio 機能なので誤り、D は Journey Builder 内の機能。

### Q3-10【単一選択】
- data-correct="B", data-type="single"
- 問題: 大規模配信（数百万件規模）を Journey 経由で実施する際に、送信スループットを向上させる仕組みは？
- 選択肢:
  - A. Random Split
  - B. High-Throughput Sending (HTS)
  - C. Tracking Extract
  - D. Suppression List
- 根拠: HTS は大規模 Journey 送信向けの高速送信モード（Spring '25 で正式化）。
- 誤答理由: A は分岐機能、C は分析、D は除外管理。

## Sidebar Navigation Update
サイドバーの「7. 練習問題 70 問」配下の各リンクは既存のまま（変更不要）。

## Quality Checklist

- [ ] page-quiz, page-quiz-section1, page-quiz-section2, page-quiz-section3 の data-placeholder 属性を削除
- [ ] page-quiz にセクション索引カード 6 枚が表示される
- [ ] 各セクションページに問題索引バー + 10 問が表示される
- [ ] 問題索引バーのリンクをクリックすると該当問題までスクロールする
- [ ] 単一選択問題で選択肢をクリックすると即座に正誤判定される
- [ ] 複数選択問題で必要数（2 つ）を選択すると自動判定される
- [ ] 判定後、全選択肢が disabled になり、再選択不可（ロック）
- [ ] 正解選択肢は accent カラー、誤答選択肢は status-stale カラーで強調
- [ ] 未選択の正解には控えめなマークが付く
- [ ] 解説（◯/× 表示 + 根拠 + 誤答理由）が表示される
- [ ] リロードすると初期状態に戻る（localStorage 使わない）
- [ ] 他のページが引き続き正常動作する

## Out of Scope
- Section 4, 5 と模試（Part 2 で実装）
- スコア集計機能
- 進捗保存