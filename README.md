# MC-202 Study Portal

Salesforce Marketing Cloud Email Specialist (MC-202) 試験対策ガイド。
社内学習用、BIGM2Y Digital Solutions Department メンバー向け。

## 公開 URL

https://yukihiro-murai.github.io/mc202-study-portal/

## コンテンツ

- **0. 前提知識**: MC 全体構造の再確認 (Marketing Cloud Engagement のアプリ構造、Contact vs Subscriber、BU 構造)
- **1. マクロ**: 試験全体の構造・配点・攻略方針
- **2. メソ**: セクション別の出題ポイント (5 サブセクション)
  - 2.1 Email Marketing Best Practices (10%)
  - 2.2 Content Creation and Delivery (24%)
  - 2.3 Marketing Automation (26%)
  - 2.4 Subscriber and Data Management (26%)
  - 2.5 Insights and Analytics (14%)
- **3. ミクロ**: 15 個の判断シナリオ
- **4. ひっかけ集**: 5 テーマ別「罠 / 正しい理解」一覧 (試験直前暗記用、モバイル最適化)
- **5. チートシート**: 判断軸まとめ (試験直前確認用、モバイル最適化)
- **6. 学習ロードマップ**: 3 ヶ月学習プラン (参考)
- **7. 練習問題 70 問**: Section 1-5 各 10 問 + 模試 20 問

## 機能

- インタラクティブクイズ (選択肢タップで即時判定 + 解説表示、複数選択対応)
- 判断シナリオの展開折りたたみ
- 学習進捗チェックリスト (リロードで初期化)
- 用語ツールチップ (ホバー / タップ)
- Markdown コピー (各セクション)
- PC / モバイル両対応
- キーボードアクセシビリティ
- ダークトーン (Muted Teal)

## 技術スタック

- HTML5 / CSS3 / Vanilla JavaScript
- 外部依存なし
- 単一 HTML ファイル
- ホスティング: GitHub Pages

## プロジェクト構成

```
mc202-study-portal/
├── AGENTS.md             # Codex CLI 向けメタファイル
├── CLAUDE.md             # Claude Code 向けメタファイル
├── README.md             # このファイル
├── IMPLEMENTATION.md     # 初回 HTML 設計仕様
├── .gitignore
├── index.html            # 全コンテンツ (単一ファイル)
└── docs/
    ├── SECTION_0_PREREQ.md
    ├── SECTION_1_MACRO.md
    ├── SECTION_2_MESO.md
    ├── SECTION_3_MICRO.md
    ├── SECTION_4_TRAPS.md
    ├── SECTION_5_CHEATSHEET.md
    ├── SECTION_6_ROADMAP.md
    ├── SECTION_7_QUIZ_PART1.md
    └── SECTION_7_QUIZ_PART2.md
```

## 更新フロー

1. `index.html` を編集
2. HTML 冒頭の `<meta name="last-updated">` を当日日付に更新
3. ブラウザで動作確認 (`open index.html`)
4. git push で GitHub Pages に自動反映

```bash
git add index.html
git commit -m "Update: <変更内容>"
git push
```

数分以内に公開 URL に反映される。

## 情報鮮度判定

TOP ページの「情報鮮度」表示は、`<meta name="last-updated">` の値と現在日時を比較して自動判定:

- 3 ヶ月以内: 良好
- 6 ヶ月以内: 通常
- 6 ヶ月超: 不良

## メンテナンス

- 更新者: Yukihiro Murai
- 連絡: TOP ページの連絡フォームから (mailto: でメールアプリ起動)

## ライセンス・利用範囲

社内利用前提。コンテンツは Salesforce 公式試験範囲の学習情報であり、社内固有情報や試験問題そのものは含まない。
