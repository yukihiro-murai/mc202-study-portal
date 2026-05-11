# Project: MC-202 Study Portal

## Purpose
社内向け Salesforce Marketing Cloud Email Specialist (MC-202) 試験対策ガイド。
GitHub Pages で配信する単一 HTML コンテンツ。
学習者は BIGM2Y Digital Solutions Department のメンバー。

## Scope
- 単一 HTML ファイル (CSS / JS をすべて内包)
- ダークトーンのデザイン (Muted Teal アクセント)
- PC + モバイル両対応
- 進捗記録機能なし
- 外部ライブラリ依存なし (フォント Webfont は許容)

## Tech Stack
- HTML5 (semantic markup)
- CSS3 (CSS Variables, Flexbox, Grid)
- Vanilla JavaScript (ES6+)
- 外部依存ゼロが原則

## Constraints
- localStorage / sessionStorage は使用しない (記録機能不要のため)
- 外部 JS ライブラリ (jQuery, React, Vue 等) を導入しない
- 単一 HTML ファイル構造を維持する
- ビルドステップを導入しない (素の HTML を直接配信)

## Coding Conventions
- HTML: 意味的構造で構造化 (section, nav, article, aside)
- CSS: CSS Variables 主体、BEM 風の命名 (.block__element--modifier)
- JavaScript: 関数単位で分離、グローバル汚染なし、IIFE か module pattern で囲む
- コメント: 必要最小限、日本語可

## Update Flow
1. ローカルで index.html を修正
2. ブラウザでローカル動作確認 (open index.html)
3. HTML 冒頭の <meta name="last-updated"> 値を当日日付に更新
4. git add / commit / push
5. GitHub Pages に自動反映 (数分以内)

## Out of Scope
- 印刷対応 (@media print)
- PDF 生成
- バックエンド連携
- ユーザー認証
- 進捗保存
