# 精査: co-working / muti-repo-workspace / mario-dogfood-prep / takt

調査: 独立サブエージェント(Sonnet)。読み取り専用。2026-08-02。
親エージェントによる裁定は docs/records/2026-08-02-github-repos-sweep.md。

## co-working
- 目的: co-working-workflow(合流ツール)の「Phase -1 Run 1」実験記録。設計本体は 2026-07-25 に co-working-workflow へ移管済み(phase-minus-1-run1.md 冒頭注記)。残るのは Run 1 の実施記録のみ
- 状態: 最終コミット 2026-07-27。records シート 0002-policy-redesign.md は未記入テンプレート(「作業日: / 合流日:」空欄)、0001 も同欄空欄 = Run 1 の合流フェーズ未実施
- 未コミット: なし
- A 候補: phase-minus-1-run1.md(自己を「Run 1 の実施記録」と明示)/ phase-minus-1-records/0001-workshop-prep-002.md / 0002-policy-redesign.md(タイトルが「Run 1 記録:」)
- 境界: merge-prompt-0001.md(合流プロトコル手順書。特定の記録ファイルに紐づく。初版 07-26 から不変)
- 既存構造: work/・docs/records/ なし。phase-minus-1-records/ というアドホック容器
- エージェント推奨: 今回導入(§12.1 移行のみ。remote 無しのため配布不要)
- **親エージェントの裁定は「Run 1 完了後に移行」**: 実験は合流フェーズ未実施の進行中で、プロトコル文書(merge-prompt)が phase-minus-1-records/ のパスを参照している。実験の途中で記録の置き場を動かすとプロトコルの参照が壊れる。remote 無しでグローバル1枚は既に有効

## muti-repo-workspace
- 目的: 複数リポジトリをまたぐサンドボックス化マルチエージェントワークスペース。checkout 中の feat/mrw は mrw 単体 CLI への切り出し改修(plan.md)
- 状態: 休眠。最終コミット 2026-07-17。feat/mrw は master +25 コミット未マージ。さらに未コミット作業が中断状態
- 未コミット: review-feedback.md の削除(D)/ rebuild-doc/(11ファイル・未追跡)/ rebuild-doc-fb/(8ファイル・未追跡)/ review-codexfb-20260716.md / review-fb-20260716.md(未追跡)
- A 候補(21本、**全て未コミットまたは削除対象**): review-codexfb-20260716.md(実施日: 2026-07-17)/ review-fb-20260716.md(実施日: 2026-07-16)/ rebuild-doc/ 11本(「2026-07-16 時点の記述」と役割宣言)/ rebuild-doc-fb/ 8本(rebuild-doc へのレビュー FB と明言)
- L: 61本(README ×2・CLAUDE.md・docs/ 36本 =「設計の正典」・templates/ 12本・.claude/skills/ 9本)。境界: plan.md(進行状況ドキュメント、8回更新)
- 既存構造: work/・records/・adr/ なし
- 推奨: 休眠につき再開時に導入(未コミット群の扱い確定が先。いま git mv すると衝突しうる)

## mario-dogfood-prep
- 目的: Mario ゲームチュートリアルのフォーク。唯一の実質コミットは「sekisho dogfood のケージ(node:22)で native build が通る toolchain 更新」(42e7d08)
- 状態: 休眠(単発タスク完了・検証済み)。コミット2件のみ(2021-01-03 初期、2026-07-19)
- 未コミット: .DS_Store のみ
- A 候補: なし。md は README.md(動作確認手順 = L)1本のみ
- 推奨: 対象外(移行対象が存在しない。文書が増える段階で再検討)

## takt
- remote: https://github.com/nrslib/takt.git(所有者 nrslib ≠ taku7777777)→ 第三者リポジトリの単純 clone(fork 経由でない)
- ローカルコミット: 0件(HEAD = origin/main = f19f339d 完全一致)。未コミットなし
- 結論: 対象外(第三者リポジトリ。配布・移行の対象にならない)
