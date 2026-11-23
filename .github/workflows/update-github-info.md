---
name: update-github-info
description: Mona のメモと GitHub の公式情報をもとに、GitHub Info サイトの更新案を作る。
intent: 開発者に役立つ GitHub の最新情報を、Mona のレビューを経てサイトに反映できるようにする。
on:
  workflow_dispatch:
  schedule:
    - cron: '17 9 * * *'
  skip-if-match: 'is:pr is:open "gh-aw-workflow-id: update-github-info" in:body'
permissions:
  contents: read
  pull-requests: read
safe-outputs:
  create-pull-request:
    title-prefix: "[mona] "
    draft: true
    fallback-as-issue: false
    allowed-files:
      - site/content/github-info.md
tools:
  github:
    mode: local
    toolsets: [repos, pull_requests]
  edit:
  web-fetch:
network:
  allowed:
    - github.blog
    - github.com
    - awesome-copilot.github.com
engine:
  id: copilot
  copilot-sdk: true
---

# Mona の GitHub Info サイトを更新する

毎日の定期実行、または手動実行で、次の手順に従って更新案を作成してください。

1. GitHub リポジトリの API ツールで `notes/mona-notes.md` と現在の
   `site/content/github-info.md` を読み、Mona の編集方針と掲載済みの情報を確認してください。
   リポジトリ内のガイドや参照ファイルを読むために、ターミナル、CLI、サンドボックス内のコマンドを使わないでください。
2. `web-fetch` で次の公式情報源を取得してください。
   - GitHub Blog: https://github.blog/latest/
   - GitHub Changelog: https://github.blog/changelog/
   - Awesome Copilot workflows: https://awesome-copilot.github.com/workflows/
   外部の公開ガイドや、掲載内容を裏付ける記事を読む場合も `web-fetch` を使ってください。
3. 各一覧で確認できる直近の情報から、掲載済みの内容と重複しない、開発者に役立つ更新を選んでください。
   Mona のメモに従い、短く実用的にまとめ、元の記事へのリンクと、どの情報源に基づく内容かを明記してください。
   記事の内容を確認できない場合は、見出しだけから詳細を推測しないでください。
4. `edit` ツールを使い、`site/content/github-info.md` だけを更新してください。
   既存の有用な情報と構成を保ち、必要な更新に範囲を絞ってください。
5. Open a pull request for Mona to review.
   `safe-outputs` の `create-pull-request` を使い、Mona または GitHub Info に言及するタイトルで
   ドラフトPRを作成してください。PR本文には、変更の要点、参照元のリンク、Mona に確認してほしい点を書いてください。
   `main` に直接書き込まず、PRを自動でマージしないでください。

同じワークフローの未完了PRがある場合、関連する新情報がない場合、必要なファイルや情報源を取得できない場合、
または変更を裏付ける情報が不足する場合は、理由を添えて `noop` を使い、PRを作成しないでください。
