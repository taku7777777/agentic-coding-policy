# 精査: claude-code-monitoring / claude-code-monitoring-ex / claude-code-sandbox-experiments

調査: 独立サブエージェント(Sonnet)。読み取り専用。2026-08-02。
親エージェントによる裁定は docs/records/2026-08-02-github-repos-sweep.md。

## claude-code-monitoring
- 目的: Claude Code 利用実態(コスト・トークン・コンテキスト)をローカル OTel→Loki→Grafana で可視化する個人向け監視基盤
- 状態: 完了(退役)。全33コミットが 2026-07-14〜17 の4日間バーストに集中し、直近16日間コミットゼロ。後継リポジトリ claude-code-monitoring-ex の README 冒頭に「旧 claude-code-monitoring(OTel→Loki→Grafana)のゼロベース後継で、旧構成は2026-07-17に退役済み」と明記
- 未コミット: docs/0-fundamentals/01-cost-model.md の変更1件(mtime 07-17、放置)
- 補足: ローカル branch feat/mrw-telemetry-network が origin/master に対し1コミット未マージ(文言置換2行のみ)
- A 候補: docs/reviews/2026-07-17-data-query-performance.md / docs/reviews/2026-07-17-feature-composition.md(役割宣言「日付: 2026-07-17 / 実測ベースの精査」・単発コミットで不変)/ docs/CASEBOOK.md(観測値の記録、初版以来不変)/ requirements.md(自己降格の経緯あり、境界事例)。docs/adr/0001〜0006 は容器内 = パスで A 確定・移行対象外
- L: 55件(docs/0-fundamentals/・1-references/・2-pipelines/・3-requirements/、CONTRACT/ONBOARDING/RUNBOOK/USECASE)
- 既存構造: work/ なし・docs/records/ なし。docs/adr/(6)と docs/reviews/(2)が自然発生の A 的構造
- 推奨: 対象外(退役済み。移行 PR のコストに見合わない)

## claude-code-monitoring-ex
- 目的: Claude Code の LLM 利用コストを「行動変容」で最適化するシステム(ccwatch)。claude-code-monitoring のゼロベース後継
- 状態: エージェント判定は「活動中(小休止)」(直近5コミットが 2026-07-21、以後12日無コミット・クリーン)
- A 候補: docs/08-dashboard-fb.md(役割宣言あり・初版以来不変・容器外)。docs/adr/0001〜0011 は容器内
- L: 30件(00-vision〜09、RUNBOOK/SCHEMA/METRICS/BENCH、docs/cli/ ほか)
- 推奨(エージェント): 導入(remote 無し → 配布不要、08-dashboard-fb.md の移行のみ)
- **親エージェントの追加検証で覆した**: docs/ ツリーが metsuke と同一構成(同名ファイル群・ADR 0011 が同一タイトルで ex 側が古い版「採択(実装前)」、metsuke 側は「更新: 07-25 / 採択・実装済み」)。ex の最終コミット日 2026-07-21 = metsuke の Initial commit 日。**ex は metsuke の前身(履歴を作り直して公開された)** → 導入・移行とも対象外。docs/08-dashboard-fb.md は metsuke 側で PR #17 により docs/records/ へ移行済みの、当時凍結の複製

## claude-code-sandbox-experiments
- 目的: Claude Code の permission / sandbox の実挙動を実測で確定する実験リポジトリ(21グループ・181サブケース)
- 状態: 休眠。実質コミットは 07-03 と 07-07 の2件のみ。未コミット作業(cases/04-devcontainer i〜l 新設・DEVCONTAINER-FINDINGS.md §6 追記・ハーネス追加)が 07-11〜12 から約21日間宙に浮いている
- A 候補(docs/ 直下・容器外): docs/FINDINGS.md(追補1〜8 を時系列追記。ただし README 索引で★= 正本扱いの節もあり境界事例)/ docs/SANDBOX-RUNTIME-FINDINGS.md / docs/DEVCONTAINER-FINDINGS.md(未コミット差分自体が A 的追記運用の証拠)
- L: 242件(README・ARCHITECTURE・GLOSSARY・SANDBOX-ENVIRONMENTS ほか、cases/**/README 同型223件)
- 既存構造: work/・records/・adr/ なし。「追補」形式が非公式に A 運用を代替(GLOSSARY が用語「追補」を定義するほど定着)
- 推奨: 再開時に導入(PUBLIC のため再開時は per-repo AGENTS.md 併置も必要。追補慣行があるため移行コストは低い)
