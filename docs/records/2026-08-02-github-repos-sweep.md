# ~/github 全リポジトリの一括スイープ — 導入・移行の全数裁定

日付: 2026-08-02

前セッション棚卸しの項目4「残リポジトリの一括スイープ」の実施記録。目的は「~/github 配下の
すべてのリポジトリが、規約導入済み or 名指しの保留理由付き、のどちらかである」状態にすること。

方法: (1) 全ディレクトリの棚卸し(git 有無・remote・可視性・dirty・最終コミット・マーカー grep ——
全リポジトリでマーカー0件)、(2) 未精査10件を独立サブエージェント3本で内容判定(生データ:
`work/2026-08-02-github-sweep/`。読まなくても本記録は自立する)、(3) 親エージェントの裁定。
判定基準は policy.md 第9版 §2.1・§12。

## 裁定(全数)

| リポジトリ | 可視性/remote | 裁定 | 根拠・条件 |
|---|---|---|---|
| agentic-coding-policy | PUBLIC | 導入済み(本体) | — |
| metsuke | PUBLIC | 導入済み | v8 到達 = PR #18(review.md §13) |
| preference-analyzer | PUBLIC | 導入済み | review.md §12 |
| co-working-workflow | PUBLIC | 導入済み | review.md §12・§13 |
| **skills** | PUBLIC | **本スイープで導入**(`9f1fba5`・本流到達済み) | AGENTS.md == §11 の diff 一致確認。容器外 A は `quality/results/<日付>/` のみで、評価ハーネスがパスを固定する成果物 = **§3.3 につき移行しない**と裁定 |
| sekisho | remote 無し | §12.1 移行を別途実施(本タスク後続) | 配布不要(グローバル1枚が有効) |
| self-analysis | PRIVATE | **保留: ユーザーの commit 後に移行** | A 候補 9/11 本のうち8本が未コミット(2026-07-10 のキャリア策定一式)。tracked 1本だけの部分移行は未コミット文書の前提参照を壊す。配布は不要(非公開) |
| muti-repo-workspace | PUBLIC | **保留: 再開時に導入** | 休眠(07-17〜)+ A 候補21本が全て未コミットの中断作業(rebuild-doc 再編)。合流確定が先 |
| claude-code-sandbox-experiments | PUBLIC | **保留: 再開時に導入** | 休眠(実質 07-07〜)+ 未コミット実験が宙。FINDINGS 群の「追補」慣行があり再開時の移行コストは低い |
| co-working | remote 無し | **保留: Run 1 完了後に移行** | 実験(Phase -1 Run 1)が合流フェーズ未実施の進行中で、プロトコル文書が `phase-minus-1-records/` のパスを参照している |
| claude-code-monitoring | PUBLIC | 対象外(退役) | 後継の README が 2026-07-17 退役を明記 |
| claude-code-monitoring-ex | remote 無し | 対象外(**metsuke の前身**) | docs/ ツリーが metsuke と同一構成、ADR 0011 が同一タイトルで ex 側が古い版、最終コミット日 = metsuke の Initial commit 日(2026-07-21)。`docs/08-dashboard-fb.md` は metsuke 側で PR #17 移行済みの当時凍結複製 |
| knowledge | PRIVATE | 対象外 | 完結した教材で A 候補0・86日無コミット |
| mario-dogfood-prep | PUBLIC | 対象外 | md は README 1本のみ・A 候補0(単発 toolchain 検証、完了) |
| takt | 第三者(nrslib) | 対象外 | 単純 clone・ローカルコミット0 |
| 非 git 6件(archived / daily-development-ops / notes / muti-repo-workspace-ex / sekisho-dogfood-artifacts / sekisho-dogfood-run) | — | 対象外 | 規約は git 前提(§7.2) |

## 帰結

- 公開リポジトリの per-repo 併置(§12.3)は **5/5 で充足**(skills を本スイープで追加。§12.2 の一覧を更新済み)
- グローバル1枚は全リポジトリに有効。保留4件はいずれも「配布」ではなく「§12.1 移行」だけが残っており、
  保留理由はすべて**未コミット/進行中の作業との衝突回避**(§12.1 の「先に合流させてから移行する」の適用)
- 保留4件の再開時チェックリスト: self-analysis(commit 後に移行)/ muti-repo-workspace(合流後に導入)/
  claude-code-sandbox-experiments(再開時に導入+PUBLIC につき配布も)/ co-working(Run 1 完了後に移行)

## 追記(2026-08-02): sekisho §12.1 移行の実施

全数55本の内容判定(生データ: `work/2026-08-02-github-sweep/sekisho-survey.md`)により
**容器外 A 39本を `docs/records/` へ移行**した。活動中11本(README / 00-final / 01-decision-log /
11・15・20・21 / p4-findings / p4-m3-plan / p5-plan / runbook)は除外。境界の裁定:
design/README.md は **L の文書表として残置**(§9。リンクのみ records/ へ更新)/
**design/16〜19 は正文15(P4-M3 進行中)の凍結後にクラスタ一括移行**として保留。

- 実施は事実上の幹 **sekisho-m1 上**: rename 39件は並行セッションのコミット `30d61ff` に混入して
  先行到達(git mv の index を並行コミットが拾った。共有ブランチのため履歴書き換えせず受容)、
  内容側(`日付:` 遡及付与・リンク機械修正・L 側参照更新)は `c33d401`。
  リンク検証: 相対 md リンク504件中 broken は移行前からの既存2件のみ
- 教訓: **並行セッション稼働中のリポジトリでは git mv(自動ステージ)とコミットの間に
  他者のコミットが挟まると rename が混入する**。次回は「mv → 即コミット」を1手で行うか静穏時に実施する
- **main への到達は次回合流時**(main は sekisho-m1 の祖先で、定期合流のマイルストーン先。
  §5.3 の完了宣言はこの点で「sekisho-m1 到達済み・main 合流待ち」と記す)
