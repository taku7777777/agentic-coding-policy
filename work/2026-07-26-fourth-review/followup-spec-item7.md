# 追加仕様 — 指摘7(intent.md)の裁定確定

`codex-spec.md` の実装完了後に適用する。編集対象: `policy.md` / `review.md` / `docs/cowork/0002-policy-redesign/intent.md`。

## 調査結果(裁定の根拠。co-working-workflow リポジトリの実測)

- `docs/cowork/<task-id>/intent.md` は cowork CLI がフック(UserPromptSubmit / Stop 等)のたびに読むファイル。パスは `cli.ts:402` でハードコードされ、設定では変更できない
- ファイルが無くてもツールはエラーにならないが、方針の版追跡(`intent-log.jsonl`)が更新されなくなる。つまり `work/` へ移すと機能が壊れる
- 生成はツールではなく手動配置(テンプレは `co-working-workflow/phase-minus-1.md`)。意図された運用は「配置 → 記入 → git 追跡」で、`metsuke/docs/cowork/0001-workshop-prep-002/intent.md` に記入済み・コミット済みの先例がある

## 1. policy.md — §3.3 を新設

§3.2 の直後(§4 の前)に追加:

> ### 3.3 例外 — ツールがパスを固定する成果物
>
> 外部ツールが読み取りパスを固定している成果物は、そのパスが権威ある場所の中でも、指定パスに置く。
> 実例: cowork の `docs/cowork/<task-id>/intent.md`(ツールはフックのたびにこのパスだけを読む。
> 移動はコード変更なしにできない)。ただし:
>
> - **未記入テンプレートのまま放置しない。** 置いたら記入し、commit する(untracked 放置は実測故障 #4 の再演)
> - **タスク完了時に冒頭へ `種別: 記録` を付す。** 以後は当時の方針の記録(A)として §6 で扱う
> - この例外は「ツールがパスを固定している」場合に限る。人が置き場を選べる成果物には適用しない

## 2. policy.md — §11 スニペットに1行追加

スニペット末尾の「作らないもの: …」の行の直前に追加:

> 例外: 外部ツールが読み取りパスを固定している成果物(cowork の `docs/cowork/<task>/intent.md` 等)は
> 指定パスに置き、記入して commit する。タスク完了時に冒頭へ `種別: 記録` を付す。

## 3. policy.md — 第5版冒頭の変更概要に反映

1.1 で挿入した「第5版の主な変更」段落の「/ §12 の観測期限を…」の前に追記:
「/ ツールがパスを固定する成果物の例外を新設(§3.3)」

## 4. review.md — §10 表の指摘7 の裁定セルを置き換え

「**採用**。裁定は生成元の調査結果を待って確定(TODO: 調査後に更新)」を次に置き換え:

> **採用**。調査の結果、cowork CLI がフックのたびに `docs/cowork/<task-id>/intent.md` を読み(パスはハードコード・設定変更不可)、`work/` への移動はツールの方針追跡を壊す。§3.3「ツールがパスを固定する成果物」を新設し、本件は記入して commit で解消(`metsuke: docs/cowork/0001-workshop-prep-002/intent.md` に「配置 → 記入 → git 追跡」の先例)。master 側チェックアウトの同名未記入コピー(untracked)はブランチ統合前に手動削除が必要

## 5. docs/cowork/0002-policy-redesign/intent.md — 記入

プレースホルダ5行を次の内容に置き換え(ラベルと行構成は維持):

> - なぜ:       LLM協働の成果物が散乱し権威の所在が不明になる問題を、規約(policy.md)で解消するため
> - 何を:       policy.md 第4版への第4巡レビューと第5版への改訂、metsuke への配布(§12 パイロット開始)
> - やらないこと: 既存散乱の移行(§1)。リポジトリ改名の判断
> - 触る範囲:   policy.md / review.md / proposal.md / issue.md / work/、および metsuke の AGENTS.md・CLAUDE.md
> - 完了条件:   第5版がコミットされ、metsuke に v5 スニペットが配布され、§12 の観測が開始されていること

記入後、`git add docs/cowork/0002-policy-redesign/intent.md` で追跡対象にする(commit はしない。まとめて後で行う)。
