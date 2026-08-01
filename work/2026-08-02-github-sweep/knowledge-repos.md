# 精査: knowledge / self-analysis / skills

調査: 独立サブエージェント(Sonnet)。読み取り専用。2026-08-02。
親エージェントによる裁定は docs/records/2026-08-02-github-repos-sweep.md。

## knowledge
- 目的: ISTQB Advanced Level Test Analyst (CTAL-TA) v4.0 学習ノート集(README「知識の集積所」)。節別詳細36本+章別一括5本+索引+README の計48本で完結した教材
- 状態: 完了。最終コミット 2026-05-08 から86日間コミットなし。シラバス全5章を欠落なくカバーし TODO なし
- 未コミット: なし(.DS_Store・.claude/ のみ)
- A 候補: なし(全48本が L = 学習リファレンス。全て初版 2026-05-07/08 から不変だが、教材という性質自体が現在形)
- 既存構造: work/・records/・adr/・CHANGELOG なし
- 推奨: 対象外(A 候補ゼロで移行対象なし。PRIVATE のため配布も不要)

## self-analysis
- 目的: 思想の整理・分析・キャリア設計(README)。AI 開発動向の一次調査、登壇・記事一覧、キャリア戦略の起草・決定
- 状態: 活動中(直近作業 2026-07-10 付。策定済みの決定文書1本と支援調査・比較検討案7本が未コミットのまま = commit 待ち)
- 未コミット: 2026-07-10_career-and-learning-plan.md + 2026-07-10_career-plan-materials/(7ファイル)
- A 候補(9/11本):
  - 2026-07-06_ai-dev-trends-and-career-forecast.md(**tracked**)— 役割宣言「調査日: 2026-07-06」、初版以来不変
  - 2026-07-10_career-and-learning-plan.md(未コミット)— 「策定日: 2026-07-10」。上記調査を「前提文書」として参照
  - career-draft-{hybrid,management,authority}.md(未コミット)— 「起草日: 2026-07-10」。3路線比較(hybrid 採択、他は実測・発見を含む不採択案)
  - career-research-{em-role,jp-career,learning,sdlc}.md(未コミット・4本)— 「調査日: 2026-07-10」+調査手段明記
- L: README.md / presentations.md(追記型の一覧)
- 既存構造: work/・records/・adr/ なし
- エージェント推奨: 今回導入(ただし未コミット8本はユーザー commit 後に移行)
- **親エージェントの裁定は「再開時(commit 後)に移行」**: A 集合の 8/9 が未コミットで、tracked 1本だけの部分移行は未コミット文書(plan が前提文書として参照)のリンクを壊す。PRIVATE のため配布は不要 —— グローバル1枚が既に有効で、移行だけが残タスク

## skills
- 目的: Claude Code 用スキル集(約20スキル×SKILL.md/evals.md/references + 評価ハーネス quality/)。PUBLIC
- 状態: 活動中(直近コミット 2026-07-31。3週間で4回の feature コミット)。working tree クリーン
- A 候補: quality/results/2026-07-20/・2026-07-31/ 配下の md(57本)— 評価実行スナップショット(1計測=1日付ディレクトリ)。ただし評価ハーネス(scripts/eval_tasks.py 等)が読み書きするパス固定の成果物
- L: SKILL.md/evals.md/references(約99本)・quality/fixtures(21本)・README
- 既存構造: work/・records/・adr/ なし。quality/results/<date>/ が独自の記録ディレクトリとして §5.6 の「1計測=1日付レコード」を自然発生的に実践
- 推奨: 今回導入(配布+移行対象なし)
- **親エージェントの裁定・実施**: 導入を実施(2026-08-02)。quality/results/ は §3.3(ツールがパスを固定する成果物)として移行しないと裁定。origin/master(4418b73)起点の scratch worktree で AGENTS.md(§11 v8)+ CLAUDE.md(@AGENTS.md)を直接 push(9f1fba5。master 保護なし = 即時本流到達)。本流上で AGENTS.md == §11 ブロックの diff 完全一致を確認。checkout 中のユーザーブランチ(codex/strengthen-skills-evaluation)には非接触
