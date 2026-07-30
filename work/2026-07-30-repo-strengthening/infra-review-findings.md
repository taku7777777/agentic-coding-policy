# リポジトリ基盤・伝達力レビュー — 全記録

日付: 2026-07-30 / 対象: /Users/takuto/github/agentic-cording-policy (master @ 14040cb)
観点: 初見の読者・エージェントに規約が正しく伝わり、参照・再利用される公開リポジトリとして何が足りないか

注: 指示された出力先「undefined/infra-review-findings.md」はオーケストレータの変数未展開で
パスとして成立しないため、本記録は scratchpad に置いた(リポジトリは調査のみ・無変更)。

---

## 1. README の30秒テスト

- README.md:1 `# agentic-cording-policy` — 見出しが typo 名そのまま
- README.md:3 「LLMと一緒にタスクを進めるときの進め方について。」 — 「進めるときの進め方」が冗長で、
  1行目としては主題(成果物の置き場・権威・寿命)を伝えない。実質の主題文は L5-6
- README.md:11 に §11 スニペット + `CLAUDE.md` は `@AGENTS.md` の導線はある。ただし表のセル内1文で、
  「導入したい人」向けの手順節(何をどこにコピーするか・3ステップ)は無い
- README に現行版(第4版 / 2026-07-25)の表示が無い。版は policy.md:5 を開かないと分からない
- 判定: 「何のリポジトリか」は L5-6 + 表で30秒以内に分かる(合格)。「導入するには何をコピーするか」は
  §11 まで潜る必要があり導線が弱い(不合格寄り)

## 2. リポジトリ名 "cording" typo — 被参照の実測と改名評価

### 実測(grep / gh / git)

リポジトリ内の自己参照(L=生きる文書のみ改名時に要更新):
- README.md:1 `# agentic-cording-policy`(L)
- policy.md:381 `<!-- artifact-policy v4 (2026-07-25) — 本体: agentic-cording-policy/policy.md -->`(L)
- proposal.md:236, proposal.md:268 / review.md:148 — いずれも A(当時形・上書き禁止)。§6 により
  当時の名前のまま残してよい(改名後も GitHub リダイレクトで主張は壊れない)

外部からの参照(実測):
- /Users/takuto/github/metsuke/AGENTS.md:1 `— 本体: agentic-cording-policy/policy.md`(配布スニペット。L)
- /Users/takuto/.claude/commands/wrap.md:5 「成果物管理ポリシー(`agentic-cording-policy/policy.md` 第4版)に従い」
- /Users/takuto/.claude/hooks/artifact-policy-log.sh:2(コメント)
- /Users/takuto/github/co-working/phase-minus-1-run1.md(4箇所)、
  co-working/phase-minus-1-records/0002-policy-redesign.md:3-4 — 記録系。当時形のまま残せる
- アクティブな worktree: /Users/takuto/tasks/0002-policy-redesign/agentic-cording-policy
  (branch 0002-policy-redesign @ 7e64e63)。ローカルディレクトリ改名はこれと
  ~/.claude/projects/-Users-takuto-github-agentic-cording-policy/(memory)を壊すため、GitHub 側改名と分離する

### 決定的事実: 正名リポジトリが既に自分名義で存在する

- gh 実測: `taku7777777/agentic-coding-policy` が PUBLIC で存在。createdAt 2026-07-25T11:56:04Z、
  pushedAt 同時刻(Initial commit の README 91バイトのみ、以後未使用)
- ローカルにも /Users/takuto/github/agentic-coding-policy/ (README.md のみ、ddbe188)
- description は「LLMと一緒にタスクを進めるときのやり方について」で本体の
  「…進め方について」とほぼ同一 — プロフィール上、中身のある typo 名と空の正名が並ぶ紛らわしい状態

### 評価(提案止まり・ユーザー判断)

- 益: 公開規約リポジトリとして名前の正しさは信頼性に直結。metsuke/AGENTS.md 等の「本体:」ポインタが
  今後増えるほど改名コストは単調増加する。今は L 側の要更新が3行(README.md:1 / policy.md:381 /
  metsuke/AGENTS.md:1)+ wrap.md:5 + フックコメント1行で最安
- コスト: (1) 空の正名リポジトリを先に削除または改名して名前を空ける (2) `gh repo rename` で
  旧名→新名のリダイレクトが自動設定され、既存 clone・co-working 内の旧 URL 言及は動き続ける
  (3) ローカルディレクトリ名・worktree は当面旧名のままで動作する(リモート URL はリダイレクト)
- リスク: リダイレクトは旧名が再取得されると壊れる(自分名義なので管理可能)
- 結論: 改名は低コストで益が上回ると評価するが、実行はユーザー判断。手順:
  1. `gh repo delete taku7777777/agentic-coding-policy`(空プレースホルダ。要確認)
  2. `gh repo rename agentic-coding-policy -R taku7777777/agentic-cording-policy`
  3. L 側3+2行の参照更新(A 文書は §6 に従い注記のみ or そのまま)

## 3. LICENSE 不在

- 実測: `gh repo view` で licenseInfo null / リポジトリに LICENSE ファイル無し
- PUBLIC リポジトリでライセンス無し = デフォルト all rights reserved。§11 スニペットは
  「各リポジトリの AGENTS.md に貼り」(policy.md:372)と複製を前提にした成果物なのに、
  第三者には複製の法的許諾が無い状態
- 提案: 文書リポジトリの標準である **CC BY 4.0** を第一候補。スニペットのヘッダが既に
  「本体: …」の出所表示を含む(policy.md:381)ため、帰属条件は運用と自然に整合する。
  スニペットの複製摩擦を完全にゼロにしたいなら CC0 1.0(帰属不要)も選択肢。
  コードがほぼ無い文書群なので MIT より CC 系が適合

## 4. 版管理 — git tag 不在と「v5 断絶」(最重要の発見)

- 実測: `git tag -l` は空。コミットは3つ(09f43ae → 4d7996f 第3版 → 14040cb 第4版)
- policy.md:381 のスニペットは `v4 (2026-07-25)` を自称。しかし配布先の実物
  metsuke/AGENTS.md:1 は `artifact-policy v5 (2026-07-26)` であり、**公開本体(origin/master)に
  第5版は存在しない**
- v5 の正体: ローカル branch `0002-policy-redesign`(worktree ~/tasks/0002-policy-redesign/…)の
  7e64e63「policy.md 第5版: 第4巡レビュー(規則の相互作用と配備実態)を反映」。origin には
  master しか無く未 push。metsuke 側 commit 5983378 で v5 スニペットが配布済み
- 差分実測(policy.md §11 v4 vs metsuke/AGENTS.md v5): 形態2 完了条件が
  「移動+参照文」の2要件 →「移動・`種別: 記録` マーカー・参照文」の3要件に変更、
  新設容器へのマーカー必須化、cowork 例外の新設 — 実質的な規約変更を含む
- 帰結: policy.md:373-374 「スニペットは本書からの複製である。本書が改訂されると配布済みのコピーは
  古くなる」という §11 の前提が**逆転**している(公開本体がコピーより古い)。metsuke から
  「本体:」ポインタを辿った読者は v4 に着地し、v5 の根拠(第4巡レビュー)を参照できない
- tag が効くか: **効く**。`git tag v4 14040cb` があれば `git show v4:policy.md` で配布時本体を復元でき、
  スニペット更新時に `git diff v4..v5 -- policy.md` で差分を機械的に提示できる。今回の
  「v5 がどこにも見えない」状態も tag 起票時に露見した。タグ付与は改訂時1回の行為で
  保守ループを持たず、§9 の「生成される索引」(根拠: 弱い)にも「定期棚卸し lint/cron」(根拠: 弱い)にも
  該当しない — §9 正当化は不要(そもそも非該当)
- 提案: (1) 0002-policy-redesign を master へマージして push(v5 を公開本体に)
  (2) `git tag v3 4d7996f && git tag v4 14040cb && git tag v5 <merge後>` を push
  (3) 以後、§11 スニペットの版更新と tag 付与を同一コミットで行う

## 5. 自リポジトリの AGENTS.md / CLAUDE.md 不在

- 実測: リポジトリ直下に AGENTS.md / CLAUDE.md 無し。.claude/settings.local.json は cowork capture
  フックのみで規約情報ゼロ。ここで作業するエージェントが work/ 規約を知る経路は
  README.md:15 の1行か policy.md 本文の通読のみ。Codex は AGENTS.md しか読まない
  (policy.md:373-374)ため、現状 Codex には何も届かない
- policy.md:372 は「これが実効性の本体」と言いながら、規約の自リポジトリ自身が配布を欠く
  (§8「規約は自分自身にも適用される」構造との不整合)
- §11 スニペットをそのまま貼れない理由(実測): スニペットは昇格先を「`docs/` 側」と書き
  (policy.md:389, 408)、容器が無ければ「`docs/adr/NNNN-<slug>.md` を新設」(policy.md:391)と指示するが、
  policy.md:242 「検討専用リポジトリでは `docs/adr/` を作らず、リポジトリ直下に A を置く(§8)」、
  policy.md:315-316 「ここでは §3.2 の『新設は `work/` と `docs/adr/`』は適用されない」と衝突する
- 自己配布の設計案(§8 構造に合わせた AGENTS.md。版ヘッダ付き複製なので §11 の
  「版付きの複製はその条件を満たす」に整合):
  - ヘッダ: `<!-- artifact-policy vN (日付) — 本体: policy.md(このリポジトリ) -->`
  - 権威ある場所 = リポジトリ直下(docs/ は作らない)
  - L = README.md / issue.md / policy.md — 直接上書きしてよい
  - A = proposal.md / review.md、および冒頭に `種別: 記録` を持つ直下ファイル — 上書き禁止、
    §6 の注記方式(当時の本文を残し「(→ ◯◯ で撤回)」+ `状態:` 行更新)
  - 新しい A は直下に名詞句ファイル名 + `種別: 記録` + `日付: / 状態:` 行(policy.md:325-326)。
    `docs/adr/` は新設しない
  - C = work/<YYYY-MM-DD>-<slug>/(命名・昇格・引用規律は §11 と同じ)
  - CLAUDE.md は `@AGENTS.md` の1行
- §9 抵触確認: AGENTS.md は生成索引でも lint/cron でもなく非該当。新設ファイル2枚は
  §3.2 の制限対象(置き場の新設)ではなく配布物であり、§11 が既に全リポジトリへの配布を規定済み

## 6. GitHub メタデータ(実測)

- `gh repo view taku7777777/agentic-cording-policy --json …` 結果:
  - description: 「LLMと一緒にタスクを進めるときの進め方について」 — 冗長・主題語(成果物/置き場/権威/寿命)ゼロ
  - repositoryTopics: null / homepageUrl: "" / licenseInfo: null / visibility: PUBLIC / default branch: master
- 提案: description を「LLM作業成果物(md)の置き場・権威・寿命を定める規約 — AGENTS.md 配布スニペット付き」
  程度に。topics: `llm` `claude-code` `codex` `agents-md` `knowledge-management` `documentation-policy` 等
- 付随: 空の正名リポジトリ(agentic-coding-policy)がほぼ同一 description で並存し、
  プロフィール閲覧者を誤誘導しうる(→ 発見2の改名判断と一体で処理)

## 7. 誤字・表記揺れ(全文書通読)

| # | 位置 | 原文 | 指摘 |
|---|---|---|---|
| 1 | issue.md:9 | 「過去に何度か対対応をしてきたが」 | 「対対応」→「対応」 |
| 2 | issue.md:14 | 「過程で得られて知見や成果物」 | 「得られて」→「得られた」 |
| 3 | review.md:73 | 「複数人の coworkd が書き込むと衝突する」 | 「coworkd」→「co-working」等の脱字 |
| 4 | README.md:1 ほか | `agentic-cording-policy` | リポジトリ名 typo(発見2) |
| 5 | README.md:3 | 「進めるときの進め方について」 | 冗長表現(gh description も同文) |
| 6 | proposal.md:39 ほか | `mrw/plan.md` | 略記 `mrw` が定義なしで初出(proposal.md:5 は正式名のみ) |
| 7 | proposal.md:114 ほか | `ccm-ex` | 略記 `ccm-ex` が定義なしで使用(正式名 claude-code-monitoring-ex は proposal.md:5) |
| 8 | (参考) | `muti-repo-workspace` | 実ディレクトリ名が muti(実測)。文書は実態に忠実だが、inventory.md:154 の孤児ファイル名は `multi-…` 綴りで、初見読者は文書側の typo と誤認しうる。脚注1行の価値あり |

注: issue.md は L(§8)なので誤字修正は上書きでよい。review.md:73 は A のため、修正するなら
§6 の例外扱い(review.md:5-7 が数値訂正で前例を作っている)をユーザーが判断する。

## 8. §9「作らないもの」との整合確認(制約対応)

本レビューの提案のうち §9 に接近するものは無し:
- git tag / AGENTS.md / LICENSE / README 改善はいずれも「生成される索引」「定期棚卸し lint / cron」に
  非該当(タグは改訂時1回、AGENTS.md は §11 が規定済みの配布物、他は静的文書)
- よって §9 の根拠の強さ(弱い/中/強い)を引いた正当化が必要な提案は本レビューには含まれない
