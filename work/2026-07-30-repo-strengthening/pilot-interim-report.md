# パイロット観測 中間監査報告(metsuke 先行適用)

日付: 2026-07-30
種別: 中間監査(C。権威なし)

**位置づけ: これは policy.md §12-2 が定める「パイロット終了時に1回だけ実行する」git 監査の先取りではない。中間読みである。** §12-5 の3値判定もここでは中間値であり、最終判定ではない。

**出力先の注記**: 監査指示の出力先パスはテンプレート変数未解決(`undefined/`)だったため、本リポジトリの規約(§4 命名・§8 検討専用リポジトリ)に従い `work/2026-07-30-pilot-interim-audit/` に解決した(本対応時の判断)。

---

## 1. §12 の観測計画(本体からの引用)

- 計装(主): policy.md:458 「観測は2系統。**主は git 監査**(パイロット終了時に1回だけ実行する。定期スキャンではない)」
- 計装(補助): policy.md:460 「補助として、ユーザーレベルの PostToolUse フックが Claude Code の Write|Edit による md 書き込みを全リポジトリで記録する(`~/.claude/artifact-policy-observation/md-writes.jsonl`)。スニペット未配布のリポジトリが対照群になる。**Codex・人手の書き込みはフックを通らない**ため、主計装はあくまで git 監査である」
- 期間: policy.md:461 「観測期間: **2週間(〜2026-08-08)または新規タスク5件の早い方**」
- 観測項目: policy.md:461 「(i) 新しい C 性の成果物が `work/` に着地するか (ii) **昇格が実際に実行されるか**(§10-5) (iii) **知見が形態2 で権威ある場所へ出るか**(§10-6)」
- リマインド: policy.md:462 「タスク完了時に `/wrap`(タスク締めのユーザーレベルコマンド。`~/.claude/commands/wrap.md`)を実行する」
- 判定: policy.md:463 「判定は3値。**着地する / 着地しない / 新規作業が発生せず観測できない**。3つ目のときは「成功」と読まないこと」

## 2. 配布状態の実測

### 2.1 配布コミット

```
$ cd /Users/takuto/github/metsuke && git log --oneline --all -- AGENTS.md CLAUDE.md
5983378 chore: artifact-policy v5 スニペットを配布(AGENTS.md 正 + CLAUDE.md は @AGENTS.md)
af31b45 成果物管理ポリシー v4 スニペットを配布(パイロット適用)

$ git log --follow --format='%h %ad %s' --date=short -- AGENTS.md | tail -5
5983378 2026-07-26 chore: artifact-policy v5 スニペットを配布(AGENTS.md 正 + CLAUDE.md は @AGENTS.md)
af31b45 2026-07-25 成果物管理ポリシー v4 スニペットを配布(パイロット適用)
```

時刻付き(§4 のログより): af31b45 = **2026-07-25 20:57**、5983378 = **2026-07-26 17:33**。

**発見(事務)**: 監査指示の文脈にある「2026-07-27 に配布済み」は git 履歴と一致しない。実際は v4 が 07-25、v5 が 07-26。また指示された `--since=2026-07-27` では流出候補が 0 件になり(§3.1 の B 参照)、監査自体が空振りする。§12-2 の記載どおり `--since=2026-07-25` を正とすべき。

### 2.2 CLAUDE.md

metsuke/CLAUDE.md:1 は `@AGENTS.md` の1行のみ。policy.md:372 「各リポジトリの **`AGENTS.md`** に貼り、`CLAUDE.md` には `@AGENTS.md` の1行を置く」と一致。

### 2.3 スニペットと本体の版ズレ(発見)

metsuke/AGENTS.md:1 は

```
<!-- artifact-policy v5 (2026-07-26) — 本体: agentic-cording-policy/policy.md -->
```

だが、本体 policy.md:5 は「更新: 2026-07-25(第4版)」で止まっており、policy リポジトリに v5/第5版への言及は無い:

```
$ git -C /Users/takuto/github/agentic-cording-policy log --oneline -3
14040cb policy.md 第4版: 第3巡レビュー(実効性の層)を反映
4d7996f 成果物管理ポリシー(第3版)を追加
09f43ae Initial commit
$ grep -n 'v5\|第5版' policy.md README.md issue.md
(ヒットなし)
```

§11 v4 スニペット(policy.md:381-434)と配布済み AGENTS.md の diff(要部):

```
1c1
< <!-- artifact-policy v4 (2026-07-25) — 本体: agentic-cording-policy/policy.md -->
> <!-- artifact-policy v5 (2026-07-26) — 本体: agentic-cording-policy/policy.md -->
11c11
> …。別名の容器を新設するなら冒頭に `種別: 記録` も置く
32c32,33
< → 完了条件は「移動した」**かつ**「参照文を書いた」の両方
> 移した先のファイル冒頭に `種別: 記録` と `日付: / 状態:` 行を付す(…)
> → 完了条件は「移動」「マーカー」「参照文」のすべて
49a51,53
> 例外: 外部ツールが読み取りパスを固定している成果物(cowork の `docs/cowork/<task-id>/intent.md` 等)は
> 指定パスに置き、記入して commit する。タスク完了時に冒頭へ `種別: 記録` を付す。
```

policy.md:377 「**スニペットは本書からの複製である。**」に対し、現状は**複製が本体を追い越している**。版ヘッダのおかげで検知はできた(§11 の設計どおり)が、本体が L として最新でない状態。

## 3. 主計装の git 監査(中間実行)

### 3.1 権威ある場所への新規 md(流出候補)

```
A) $ git log --since=2026-07-25 --diff-filter=A --name-only --pretty=format: -- '*.md' | sort -u | grep -v '^work/'
AGENTS.md
CLAUDE.md
docs/claude-code-cost-workshop.md
docs/cowork/0001-workshop-prep-002/intent.md

B) 同コマンド --since=2026-07-27
(0件)
```

参考(work/ 含む全新規 md、--since=2026-07-25):

```
AGENTS.md
CLAUDE.md
docs/claude-code-cost-workshop.md
docs/cowork/0001-workshop-prep-002/intent.md
work/2026-07-27-auto-mode-cost-attribution/findings.md
```

### 3.2 untracked

```
$ git status --porcelain | grep '^?? '
?? work/2026-07-27-workshop-member-metrics/
```

work/ 外の untracked は 0 件。ただし work/ 内の1ディレクトリが 07-27 21:07 作成のまま3日間 uncommitted(policy.md:303 「**`work/` は commit する。理由は「失われないため」である。**」に照らすとリスク)。

### 3.3 work/ の中身と §4 準拠

```
$ find work -type f
work/2026-07-27-auto-mode-cost-attribution/findings.md          (tracked, committed)
work/2026-07-27-workshop-member-metrics/metric-assessment.md    (untracked)
```

- 命名: `<YYYY-MM-DD>-<slug>` 形式、slug は名詞句 4語/3語 — §4 準拠
- ファイル名 `findings.md` / `metric-assessment.md` = `<内容>.md` — §4 準拠

### 3.4 docs/ 側の変更(昇格の痕跡)

```
$ git log --since=2026-07-25 --name-only --format='COMMIT %h %ad %s' (抜粋)
COMMIT ef5a2b7 2026-07-27 08:19 docs: Auto判定の拒否が観測できることを実測に基づき反映する
  docs/METRICS.md  docs/claude-code-cost-optimization.md  work/2026-07-27-auto-mode-cost-attribution/findings.md
COMMIT f24ecef 2026-07-27 07:28 docs: Auto modeのコスト計上を実測に基づき修正しCost API未使用へ統一する
  docs/02-data-model.md  docs/METRICS.md  docs/claude-code-cost-optimization.md
  docs/claude-code-cost-workshop.md  docs/cli/commands.md  work/2026-07-27-auto-mode-cost-attribution/findings.md
COMMIT fbcfbb7 2026-07-25 23:51 docs: コスト理解ワークショップの進行台本と参加者向け軽量CLIを追加する
  docs/claude-code-cost-optimization.md  docs/claude-code-cost-workshop.md  scripts/cc-cost-peek.py  tests/test_cc_cost_peek.py
```

**昇格・形態1 の実行を確認**: work/findings.md と docs 側 6 ファイルが同一コミットで変更され、findings.md:196 に「反映先: `docs/claude-code-cost-optimization.md` / `docs/METRICS.md`(§15を「オプション」化し§16を新設) / `docs/02-data-model.md` / `docs/SCHEMA.md` / `docs/cli/commands.md` / `docs/claude-code-cost-workshop.md`。」と記録されている。知見の実体も docs 側に着地している(docs/METRICS.md:237 「**罠(`source`は判定結果ではない)**: 拒否3件の`source`は`config`で、自動承認179件と同じ値。」)。

**§3.1 不変条件の確認**: docs/・README.md から `work/` への参照は 0 件(grep 実行、ヒットなし)。work/ を消しても docs の主張は壊れない状態が維持されている。

### 3.5 流出候補の内容判定(機械でなく内容で)

**(1) docs/claude-code-cost-workshop.md(442行)**

- 作成: Claude Code の Write、**2026-07-25 21:57:56** = v4 配布(20:57)の**1時間後**。commit は fbcfbb7(23:51)
- 内容: :9 「このファイルは進行台本です。記事は参照リファレンスとして残し、当日はこの順で進めます。」— 特定ワークショップの進行台本。:54 「掲載しているのは2026-07-25時点の一例で」等、単発イベント・ファシリテーター個人環境依存の記述を含む
- 同型の先例: 棚卸し(work/2026-07-25-artifact-policy/inventory.md:40)は `docs/claude-code-cost-optimization.md` を「0件。特定の登壇向け原稿」として C 判定し移行先 `work/2026-07-25-cost-optimization-talk/` を挙げていた。**同じジャンルのファイルが、スニペット配布後に再び docs/ 直下へ新規作成された**
- 一方で L 側の材料: f24ecef(07-27)で実測に基づく事実修正を受けており、**維持されている**。また docs/claude-code-cost-optimization.md から被参照になった
- **中間判定: C 性が強いが L 解釈の余地が残る灰色。** work/ を経由しなかった事実は確定(07-25 の metsuke への md 書き込みは outside 24 件 / work 0 件)。§10-1 「スニペット1枚で置き場が変わる」への反例候補として終了時監査で要精査。なお書き込みセッションが配布(20:57)より前に開始されておりスニペット未読だった可能性は、本監査のデータからは排除も確認もできない

**(2) docs/cowork/0001-workshop-prep-002/intent.md(5行)**

- 外部ツール cowork が読み取りパスを固定する成果物。a98e37b(07-26 09:07)で追跡開始 → その後 v5 スニペット(07-26 17:33)が「例外: 外部ツールが読み取りパスを固定している成果物(cowork の `docs/cowork/<task-id>/intent.md` 等)」条項を追加。**例外条項が実例の後から追加された(事後追認)**
- v5 例外の完了手順「タスク完了時に冒頭へ `種別: 記録` を付す」は未実施(intent.md:1-5 にマーカー無し)。ただし intent.md:5 の完了条件は「私のレビュー完了」で、完了したかが git から観測できないため**違反確定ではない**。終了時監査のウォッチ項目

## 4. 補助計装の分析

### 4.1 フック定義と捕捉範囲

~/.claude/settings.json の PostToolUse に matcher `"Write|Edit"` で `~/.claude/hooks/artifact-policy-log.sh` が登録されている。スクリプトは tool_input.file_path が `.md` で終わる場合のみ `{ts, cwd, tool, file}` を追記する(artifact-policy-log.sh:11-12)。

**捕捉するもの**: Claude Code(サブエージェント含む)の Write / Edit による .md 書き込み。実測でもログの tool は Edit 352 / Write 61 の2種のみ。

**漏らすもの(スクリプト自身も :4 で自認)**:
- Codex の書き込み(MCP 経由・CLI 直叩きとも)。本ユーザーは実装を Codex に委譲する運用のため、実装系タスクの md はここに映らない
- 人手の編集
- Bash 経由の書き込み(`cat >`, heredoc, `sed -i` 等)
- **`mv` / `git mv` によるファイル移動 — すなわち昇格・形態2 の実行そのもの**。観測項目 (iii) の主対象である形態2は「work/ の外へ移す」操作であり、Write|Edit を発火させない。補助計装は (iii) を原理的に観測できない(主計装の git 監査では rename として追える。終了時監査では `git log -M --diff-filter=R` の併用が必要)
- .md 以外の成果物(policy.md:89 「**文書以外も同じ。**」の対象外)

### 4.2 クロス集計(リポジトリ × work内外、全413行)

```
$ jq -r '[(.cwd|split("/")|last), (if (.file|test("/work/")) then "work" else "outside" end)] | @tsv' md-writes.jsonl | sort | uniq -c
 242 sekisho                 outside
  60 co-working-workflow     outside
  46 metsuke                 outside
  34 agentic-cording-policy  outside
  14 co-working              outside
  12 metsuke                 work
   2 co-working-workflow     work
   2 agentic-cording-policy  work
   1 0003-state-restructure  work
```

metsuke の日別:

```
  24 2026-07-25 outside     ← ワークショップ台本タスク。work 0 件
  22 2026-07-27 outside     ← 大半が findings の docs 反映(= 形態1 の書き込み)
  12 2026-07-27 work        ← findings.md(11件)+ metric-assessment.md(1件)
```

### 4.3 対照群比較

- sekisho は AGENTS.md / CLAUDE.md とも存在しない(未配布)ことを確認。**未配布 sekisho: outside 242 / work 0(work 使用率 0%)**。配布済み metsuke: 07-27 以降 work 12 / outside 22
- ただしこの比較は弱い。(a) outside には正当な L 保守・形態1 昇格の書き込みが含まれ、流出と区別できない。(b) sekisho には独自の docs/design 規約があり、outside 書き込みが規約違反とは限らない。着地率の差は「スニペットが work/ の使用を生んだ」ことの示唆に留まる

### 4.4 /wrap の実行有無

`~/.claude/commands/wrap.md` は存在する(2026-07-25 21:00 作成)。ただし実行の痕跡は git にも md-writes.jsonl にも残る設計になっておらず、**/wrap が使われたかはこの計装では観測できない**。07-27 の形態1昇格が /wrap 起点かは不明。

## 5. §12-3 観測項目への中間所見と §12-5 中間判定

| 項目 | 中間所見 | 3値の中間判定 |
|---|---|---|
| (i) C が work/ に着地するか | 07-27 の2タスクは §4 準拠の命名で work/ に着地(findings.md は commit 済み)。ただし v4 配布1時間後の 07-25 タスクは work/ を一切使わず docs/ 直下に 442 行を新規作成 | **着地する(07-27 以降)**。ただし n=2 で、配布直後の反例候補1件あり |
| (ii) 昇格が実行されるか | 形態1 が1件実行された(findings.md → docs 6ファイル、同日)。§10-5 「最初に破られるのはここである」は n=1 では破られていない。/wrap の関与は観測不能 | **着地する(n=1)** |
| (iii) 知見が形態2で出るか | 形態2 は 0 件。findings.md の知見は「厚い形態1」(§16 新設を含む大規模書き換え)で docs/METRICS.md 等に着地し、1文圧縮による喪失は起きていない。findings.md 自体は work/ に残置(§3.1 不変条件は維持) | **観測できない(発生0件)**。「成功」と読まない |

**総合(中間・最終判定ではない)**: 「着地する」寄り。ただし (a) 観測 n が小さい(新規タスク2件/5件、期間の中日)、(b) 07-25 の同型流出候補1件が §10-1 の前提を脅かす、(c) (iii) は未発生。終了時監査(〜2026-08-08 または新規タスク5件)を §12-2 記載の `--since=2026-07-25` で実行すること。

## 6. 発見事項一覧(severity 順)

1. **[medium] スニペット v5 が本体 policy.md(v4)を追い越している** — §11 「スニペットは本書からの複製である」の逆転。本体を第5版に更新し v5 の3変更を取り込むべき
2. **[medium] v4 配布1時間後に同型流出候補(docs/claude-code-cost-workshop.md、442行)** — work/ 不経由。L/C は灰色で終了時に内容確定
3. **[medium] 補助計装は形態2(移動)・Bash 書き込み・Codex・人手を捕捉しない** — 特に (iii) の観測手段が主計装の rename 検出に一本化される。終了時監査に `-M --diff-filter=R` を含めること
4. **[low] 昇格・形態1 が実行された(順調)** — findings.md → docs 6ファイル、§3.1 維持、§4 準拠
5. **[low] 形態2 は 0 件(未発生。失敗とは読まない)**
6. **[low] intent.md の完了時マーカー未付与(タスク完了未確認のため違反確定ではない)** — v5 例外条項の事後追認という経緯も記録
7. **[low] work/2026-07-27-workshop-member-metrics/ が3日間 untracked** — §7.2 に照らし commit を推奨
8. **[low] 監査基準日のズレ** — 「07-27 配布」は誤り(v4=07-25、v5=07-26)。--since=2026-07-27 では流出候補が0件になる

## 7. 使用した主なコマンド

```sh
git -C /Users/takuto/github/metsuke log --oneline --all -- AGENTS.md CLAUDE.md
git -C /Users/takuto/github/metsuke log --since=2026-07-25 --diff-filter=A --name-only --pretty=format: -- '*.md' | sort -u | grep -v '^work/'
git -C /Users/takuto/github/metsuke status --porcelain | grep '^?? '
git -C /Users/takuto/github/metsuke log --since=2026-07-25 --name-only --format='COMMIT %h %ad %s' --date=format:'%Y-%m-%d %H:%M'
grep -rn 'work/' docs/ README.md   # metsuke 内、§3.1 検証
sed -n '381,434p' policy.md > snippet-v4.md && diff snippet-v4.md /Users/takuto/github/metsuke/AGENTS.md
jq -r '[(.cwd|split("/")|last), (if (.file|test("/work/")) then "work" else "outside" end)] | @tsv' ~/.claude/artifact-policy-observation/md-writes.jsonl | sort | uniq -c
jq -r 'select(.cwd|test("metsuke")) | [.ts, .tool, .file] | @tsv' ~/.claude/artifact-policy-observation/md-writes.jsonl
```
