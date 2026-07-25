# 棚卸し表 — 既存散乱の移行候補

`policy.md` §1 の方針により、**この表は参照用であり、移行は実行しない。**

この表自身が生成一覧であり、生成時点で凍結されてリポジトリの変化に追随しない。したがって
`policy.md` §9 の「生成される索引は作らない」に従い、権威ある場所ではなく `work/` に置く(= C)。

作成日: 2026-07-25 / 第2版(独立レビューによる事実検証を反映) / 対象: 実測リポジトリ + `~/github/` 直下

**公開時の匿名化**: 非コードの個人リポジトリ2本を `priv-A` / `priv-B` と表記し、内容が特定できる
ファイル名・引用を `<…>` で伏せた。本数・行数・git状態・日付は実測値のまま。

---

## 0. 適用したルール

本表は次の規則で A / C を判定した。これは `policy.md` §5.1 の**用途(b)「既存ファイルの遡及調査」**に当たる。

> 権威ある場所(L)から、維持する理由を付して参照されているものを A とみなす。

**§5.1 はこれを判定ではなく手がかりと位置づけている。** 参照が「維持する理由」かどうかは読まないと
決まらず、参照元が単に更新されていないだけの場合を区別できない(§5-5 の `mrw/plan.md` が実例)。
したがって本表の A / C は暫定であり、最終判断は中身を読んで下す。

**第1版はこの規則の適用が不完全だった。** 検索範囲が狭く、5本を誤って C 判定していた(§3B)。
第2版では `grep -rn` を権威ある場所の**全体**(ルート含む)に対して実行し直している。

**判定は機械的ではない。** 参照の有無は grep で分かるが、それが「維持する理由」かは読まないと決まらない。
判別基準は `policy.md` §5.1 の表による。

---

## 1. C 判定(移行候補)

### metsuke

| 現在のパス | 行数 | git状態 | L→C 参照 | 移動先 |
|---|---|---|---|---|
| `docs/09-dashboard-implementation-plan.md` | 261 | tracked | **0件**(リポジトリ全体で被参照ゼロ。`README.md:95-103` の文書表にも無い) | `work/2026-07-21-dashboard-v1-review/` |
| `docs/claude-code-cost-optimization.md` | 596 | tracked/**modified** | 0件。特定の登壇向け原稿(`:11`「発表当日は、次を最新の状態へ更新してください。」) | `work/2026-07-25-cost-optimization-talk/` |
| `docs/claude-code-cost-optimization-script.md` | 417 | tracked/**deleted**(未commit) | — | **対応不要**。delete を commit すれば完了 |

### muti-repo-workspace

| 現在のパス | 本数 | 行数 | git状態 | 移動先 |
|---|---|---|---|---|
| `review-fb-20260716.md` | 1 | 84 | **untracked** | `work/2026-07-16-feat-mrw-review/` |
| `review-codexfb-20260716.md` | 1 | 189 | **untracked** | `work/2026-07-16-feat-mrw-review/` |
| `review-feedback.md` | 1 | 266 | tracked/**deleted**(未commit) | **対応不要**。delete を commit すれば完了 |
| `rebuild-doc/`(README + 01〜**10**) | **11** | **2,305** | **untracked** | `work/2026-07-16-rebuild-doc-pack/rebuild-doc/` |
| `rebuild-doc-fb/`(README + 01〜07) | 8 | 494 | **untracked** | `work/2026-07-16-rebuild-doc-pack/rebuild-doc-fb/` |

**このリポジトリが最大の移行対象**: **21本・3,072行、全て untracked** のまま 8〜9日放置
(表中の `review-feedback.md` は tracked/deleted で、この21本には含まない)
(`rebuild-doc-fb/` は 07-16、`rebuild-doc/` は 07-17)。
`docs/` 配下 **36本**(直下30 + `settings-reference/` 6、ja/en 対)は全て L で配置適正。

### notes

| 現在のパス | 行数 | L→C 参照 | 移動先 |
|---|---|---|---|
| `20260706_case-readme-sufficiency-review.md` | 150 | **0件** | `work/2026-07-06-case-readme-sufficiency-review/` |
| `20260706_purpose-based-agent-architecture.md` | 466 | 0件 | `work/2026-07-06-purpose-based-agent-architecture/` |
| `20260706_sufficiency-and-operations-review.md` | 179 | 0件 | `work/2026-07-06-sufficiency-and-operations-review/` |

実測: `GAPS-BACKLOG.md` `TAXONOMY-GAPS.md` からの参照0件。唯一のヒットは
`20260706_purpose-based-agent-architecture.md:465` で、これは C→C なので該当しない。

**注意**: `notes` は git リポジトリではない。`policy.md` §7 により、ここでは `work/` の中身を消さない。

### priv-A

| 現在のパス | 本数 | 行数 | 移動先 |
|---|---|---|---|
| `2026-07-10_<主題>-materials/` | 7 | 1,469 | `work/2026-07-10-<slug>/` — **改名のみ** |

**境界例**。同リポジトリの `2026-07-10_<主題>-plan.md:316`(A)から参照されているが、参照文は
「*中間成果物: <下書き3案>・<支援調査4本>は … に保存*」(原文は斜体のみ)であり、**参照元が対象を自ら
「中間成果物」と性格づけている**。`policy.md` §5.1 の表では「該当しない」側 → **C のまま**。

`-materials` という命名で既に「道具」と自認しており、7本が1タスク分としてまとまっている。
**最も低コストな移行対象**(改名のみ)。

### priv-B

C 判定 0件。`<教材>/` 配下 **47本** + ルート `README.md` 1本。全て L で配置適正。

---

## 2. 移行対象の総計

| repo | 本数 | 行数 | git状態 |
|---|---|---|---|
| muti-repo-workspace | 21 | 3,072 | **全て untracked** |
| priv-A | 7 | 1,469 | 全て untracked |
| notes | 3 | 795 | git 管理外 |
| metsuke | 2 | 857 | tracked(1本 modified) |
| sekisho | 0 | — | (第1版で4本を誤判定。§3B) |
| ccm-ex | 0 | — | (第1版で1本を誤判定。§3B) |
| priv-B | 0 | — | — |

**合計 33本 / 約6,193行。** うち **31本(93.9%)** が untracked または git 管理外。

第1版は「36本 / 約7,150行 / 26本(72%)」としていた。第1版の集計は本数・行数とも自表の行値と
合っておらず(19本と書いて内訳は20本、26本と書いて内訳は29本)、差分を再現できない。
第2版は各行から積み直した値であり、内訳との算術が一致することを確認してある
(21+7+3+2=33 / 3,072+1,469+795+857=6,193 / untracked 21+7+3=31)。

---

## 3. ルール適用で C から A に覆ったもの

### 3A. 第1版で覆したもの(16本 / 2,662行)

| 対象 | 根拠(実測) |
|---|---|
| `metsuke: docs/08-dashboard-fb.md` | `08-dashboard.md:620` §17「`08-dashboard-fb.md`への対応」の対応表 |
| `sekisho: docs/design/synthesis-draft.md` | `README.md:57`「反映前ドラフト(**透明性のため保存**)」+ `00-final-design-plan.md:9` |
| `sekisho: docs/design/briefs/` 5本 | `README.md:29,33-37`「解決策非依存の入力(既存資産から蒸留)」+ 各ファイルに目的 |
| `sekisho: docs/design/candidates/` 4本 | `README.md:39,43-46`「4案の独立設計(バイアス排除)」 |
| `sekisho: docs/design/review/` 4本 | `README.md:48,52-55`「検証記録」+ 各ファイルに内容 |
| `sekisho: docs/p0-spike-plan.md` | **ルートの** `sekisho/README.md:41`「P0 スパイクの実施計画・ゲート基準」(他の行は `docs/design/README.md`。参照先ファイルが違う) |

第1版は「18本」としていたが数え間違い。正しくは16本(行数2,662は正)。

また第1版は fb の根拠に `09-dashboard-implementation-plan.md:6` も挙げていたが、
**同じ表が `09-plan` 自身を C と判定している**。C からの参照は §5.1 に該当しないので根拠から外す。
`08-dashboard.md:620` だけで A は成立する。

### 3B. 第2版で発見した第1版の判定ミス(5本 / 1,130行)

**原因は2種類ある。** sekisho の4本は検索範囲が `docs/*.md` 止まりで `docs/design/**` を見ていなかったこと。
ccm-ex の1本はルート `README.md` を見ていなかったことで、`docs/design/**` とは無関係。

| 対象 | 第1版 | 正 | 根拠(実測) |
|---|---|---|---|
| `sekisho: docs/p1-plan.md` | C(0件) | **A** | `docs/design/01-decision-log.md:97,118`(`sekisho/README.md:39` が「設計決定ログ」として文書表に掲載 = L)。補強: `design/02:3` `03:3` `04:3` `05:3` がいずれも「出典: p1-plan.md」 |
| `sekisho: docs/p2-plan.md` | C(0件) | **A** | `design/01-decision-log.md:359`「計画 = ../p2-plan.md」。※ `docs/p4-plan.md:117`「— p2-plan §3 承継)。」は通りすがりの言及で §5.1 の「該当しない」側。根拠にならない |
| `sekisho: docs/p3-plan.md` | C(0件) | **A** | `docs/design/01-decision-log.md:406,523,554`。補強: `design/07:3` `08:3` の「出典:」、`10:4` の「根拠:」 |
| `sekisho: docs/p4-m2-plan.md` | C(0件) | **A** | `docs/p4-plan.md:158`「**実施計画 = p4-m2-plan.md**」。補強: `design/01-decision-log.md:782`「実施計画の全文は ../p4-m2-plan.md を**正とする**」 |
| `ccm-ex: docs/09-dashboard-implementation-plan.md` | C | **A** | `README.md:130` が文書表に目的付きで掲載(「Stage 8の依存順、変更対象、テスト、安全性gate、rollout計画」)。**metsuke 側の README にはこの行が無い**ため判定が分かれる。両者は同名・同行数(261行)だが内容は同一ではない(`diff` で20行相違。大半は `ccwatch`→`metsuke` の改名だが :54 :67 は実質的な文言差) |

**この5件は、ルールが正しくても適用が不完全なら誤るという実例である。** §5.1 の
「検索範囲は権威ある場所の全体にすること」はこの失敗から追記された。

---

## 4. リポジトリ外の孤児(`~/github/` 直下)

| ファイル | サイズ | mtime | 帰属先 | 対応 |
|---|---|---|---|---|
| `muti-repo-workspace-vs-takt.md` | 14.2KB | 2026-07-05 | **無し**(2リポジトリ構成比較) | ★**検討専用リポジトリ新設の候補**。ただし `multi-repo-workspace-improvement-plan.md:4` から「前提資料」として参照されている(孤児同士の依存) |
| `devcontainer-orchestrator-architecture.md` | 21.1KB | 2026-07-09 | muti-repo-workspace | ⚠ **`docs/devcontainer-status.md:3` と `docs/devcontainer-status.ja.md:3` の2本**が相対リンクで参照。孤児に見えて生きた依存。移動はリンク修正とセット |
| `multi-repo-workspace-improvement-plan.md` | 11.4KB | 2026-07-05 | muti-repo-workspace | 要判断(§5-4) |
| `claude-code-security-architecture.md` | 19.0KB | 2026-07-06 | **複数候補**。Claude Code の権限制御解説で mrw とは無関係 | 要判断(§5-3) |

内容上 `muti-repo-workspace` に帰属するのは **2本**(第1版は3本としていたが誤り)。

---

## 5. 要判断

| # | 対象 | 迷う理由 |
|---|---|---|
| 1 | `sekisho: docs/session-2026-07-17-progress.md`(234行) | 決定・実測事実のサマリ(A的)と、次セッションへのTODO(C的)が同一ファイルに混在。`policy.md` §10-3 の「3分類の残余」の実例 |
| 2 | `sekisho: docs/p2-live-runbook.md`(129行) | README「残(P2ゲート必須でない): 段B」で今も参照。段Aは実施済み・段Bは未実施で、C か L か進捗が中途半端で決まらない |
| 3 | `~/github/claude-code-security-architecture.md` | `priv-B`(教材として)と `claude-code-sandbox-experiments/docs/ARCHITECTURE.md`(内容重複)の両方に理由があり、逐条比較なしには決められない |
| 4 | `~/github/multi-repo-workspace-improvement-plan.md` | `mrw/plan.md`(2026-07-16〜17、`mrw` CLI化)と主題が異なるように見えるが、後者から前者への参照が無く、独立に生きているのか陳腐化しているのか断定できない |
| 5 | `mrw: plan.md`(207行) | `docs/mrw-cli.md` `docs/mrw-cli.ja.md` `docs/mrw-chat.md` `docs/mrw-chat.ja.md` の**4本**から現状の実装状況源として参照されており、ルール上は **A**。しかし内容は消費済みの計画に見える。**ルールと内容判断が食い違う唯一の実例**(`policy.md` §10-2) |
| 6 | `notes: GAPS-BACKLOG.md` / `TAXONOMY-GAPS.md` | 中身は `claude-code-sandbox-experiments` と `archived` を追跡する文書。`notes/` 自体が git リポジトリでないため、別種の判断 |
| 7 | `priv-A: 2026-07-10_<主題>-plan.md` | 本文に定期的に内容を見直す旨の記述があるが、見直し時に上書き(L)か新しい日付ファイルとして積む(A)かが未確定 |

---

## 6. 対象外とした領域

| 領域 | 理由 |
|---|---|
| `sekisho/spikes/`(**40本**、全て tracked) | `.gitignore:1,13` が「RESULTS.md is the committed summary」「FINDINGS.md summaries are committed」と md の終端規約を明記済み |
| `mrw/tasks/` `mrw/repositories/` | E2Eテスト用のデモ fixture。文書成果物ではない |
| `takt/builtins/` `takt/src/shared/prompts/`(**401本**) | 出荷される製品コンテンツ。散乱ではない |
| `~/github/archived/` | 既に C の墓場として機能している(`policy.md` §7) |
| `ccm-ex` 全体 | 2026-07-21 で凍結。`metsuke/README.md:131` が `ccwatch`(= 本リポジトリの README:1 の名称)を移行元と記載。移動より **README にアーカイブ済みと明記する方が優先度が高い** |
