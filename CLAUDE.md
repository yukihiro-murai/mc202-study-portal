# Project: MC-202 Study Portal

## Purpose
社内向け Salesforce Marketing Cloud Email Specialist (MC-202) 試験対策ガイド。
GitHub Pages で配信する単一 HTML コンテンツ。
学習者は BIGM2Y Digital Solutions Department のメンバー。

## Scope
- 単一 HTML ファイル (CSS / JS をすべて内包)
- ダークトーンのデザイン (Muted Teal アクセント、#5eb8a0 を基調)
- PC + モバイル両対応 (レスポンシブ)
- 進捗記録機能なし
- 外部ライブラリ依存なし

## Tech Stack
- HTML5 (semantic markup)
- CSS3 (CSS Variables, Flexbox, Grid)
- Vanilla JavaScript (ES6+)
- 外部依存ゼロが原則

## Constraints
- localStorage / sessionStorage は使用しない
- 外部 JS ライブラリを導入しない
- 単一 HTML ファイル構造を維持
- ビルドステップを導入しない

## Coding Conventions
- HTML: semantic markup (section, nav, article, aside)
- CSS: CSS Variables 主体、BEM 風命名
- JavaScript: 関数分離、IIFE か module pattern、グローバル汚染回避
- インデント: スペース 2 文字
- コメント: 必要最小限、日本語可

## Design Principles
- ミニマリスト志向、装飾は最小限
- ダークトーン (背景 #0a0a0a / surface #171717 / border #262626)
- テキスト primary #fafafa / secondary #a1a1aa
- アクセントカラー Muted Teal #5eb8a0
- 強い色やビビッドな装飾は避ける
- アイコンは線画のみ、過剰使用しない
- 余白を多めに取る

## Content Structure
ガイドは以下のページ構成:
1. TOP (メタ情報、注意喚起、サイドバーから各ページへ)
2. 前提知識 (MC 全体構造)
3. マクロ (試験全体構造)
4. メソ (5 セクション別出題ポイント)
5. ミクロ (判断シナリオ)
6. ひっかけ集
7. 判断軸チートシート
8. 3 ヶ月学習ロードマップ
9. 練習問題 70 問

## Sidebar Navigation
- 常時固定、左サイド、幅 240-280px
- 独立スクロール
- モバイルではドロワー化 (ハンバーガーで開閉)
- 練習問題セクションは展開可能で、Section 内の各問題にも直接ジャンプ可能

## Quiz Behavior
- 選択肢タップで即座に正解 / 不正解 + 解説を表示
- 進捗記録なし、リロードで初期状態に戻る
- サイドバーから任意の問題に直接ジャンプ可

## Copy to Clipboard
- 各主要セクションに「コピー」ボタン配置
- Markdown 形式で出力 (見出し / 表 / コードブロック保持)
- ページ全体コピーは不要、表示中のセクション粒度のみ

## Mobile Support
- 全ページレスポンシブ対応
- ブレイクポイント: 768px
- サイドバーはドロワー化
- テーブルは横スクロール可能化

## Update Flow
1. index.html を編集
2. HTML 冒頭の <meta name="last-updated"> を当日日付に更新
3. ブラウザで動作確認
4. git add / commit / push

## Out of Scope
- 印刷対応
- PDF 生成
- バックエンド連携
- ユーザー認証
- 進捗保存
