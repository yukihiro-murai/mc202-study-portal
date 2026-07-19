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

---

## マルチマシン Git 同期プロトコル (必須・スキップ禁止)

このリポジトリは Mac mini と MacBook Pro の2台で並行運用されている。**GitHub (origin) が唯一の正であり、ローカルは常に古い可能性がある。** すべての AI エージェント (Claude Code / Codex / Grok Build / Hermes Agent / Cursor 等) は以下を守ること。

- **作業開始時 (必ず実行)**: `git branch --show-current` でブランチ確認 → `git status` → `git fetch origin` → behind なら `git pull --ff-only`。fast-forward できない・未コミット変更と衝突しそうな場合は、勝手に merge / rebase / stash せず状況を報告して指示を待つ。
- **作業終了・中断時 (必ず実行)**: 変更を commit (WIP でも commit する) → `git pull --ff-only` → `git push origin <branch>` (新規ブランチは `-u`)。**push を完了するまで「作業完了」と報告しない。** push していない作業はもう一台から存在しないのと同じ。
- **禁止**: force push (ユーザー明示指示時を除く) / fetch 省略での着手 / push 省略での完了報告 / コンフリクトの独断解決。
- git push 後、`.clasp.json` があれば `clasp status` → `clasp push` で GAS に反映する。
