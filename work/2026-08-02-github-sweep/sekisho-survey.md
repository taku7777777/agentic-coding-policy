# 精査: sekisho(§12.1 移行のための容器外 A 全数調査)

調査: 独立サブエージェント(Sonnet)。読み取り専用。2026-08-02。
裁定・実施記録は docs/records/2026-08-02-github-repos-sweep.md の追記節。

## 全数(55本)の内訳

- **A 候補 39本**(全て `日付:` 行なし・マーカー0件・basename 衝突なし):
  - docs/design/: 02〜10・12〜14 の契約12本(ステータス「確定(日付)」宣言+凍結)
  - docs/design/briefs/ 5本(2026-07-17 の設計競技入力。単一コミット不変)
  - docs/design/candidates/ 4本(設計競技4案。min-tcb が採用案の原型、他3本は不採択案)
  - docs/design/review/ 4本(candidates-summary / independent-review / judges / redteam —— 明示的レビュー宣言)
  - docs/design/synthesis-draft.md(00-final に「透明性のため保存」と明記)
  - docs/ 直下 13本(p0〜p3 の findings・plan・runbook、p4-plan・p4-m2-plan、session-2026-07-17-progress)
- **活動中 11本(除外)**: README(状態板)/ 00-final-design-plan(正典)/ 01-decision-log(「以後の設計変更はこのログに追記する」= L の現在形宣言。D-53/D-54 が 08-01 追記)/ 11・15・20・21(進行中契約)/ p4-findings(継続台帳)/ p4-m3-plan / p5-plan(実測台帳)/ runbook
- **境界 5本**:
  - docs/design/README.md → 裁定 **(a) 残置**(§9「手で維持する README の文書表」= L 側の受け皿。リンクのみ records/ へ更新。records/ 内 README は §3.2 で禁止)
  - 16〜19(P4-M3 spec 群)→ 裁定 **保留**(正文 15 が活動中。15 凍結後にクラスタ一括移行 —— 生きた契約から死んだ記録への非対称参照を作らないため)

## ブランチ・並行作業の前提

- 本流関係: main は sekisho-m1 の祖先(merge-base 4c01974)。sekisho-m1 が事実上の幹で、
  records/ 新設(4bbd4cb)もこのブランチ上。未マージ差分は活動中5ファイルのみで A 候補39本に非接触
- 並行セッションが P5 作業中(コード側 internal/chatui・spikes/p5 が未コミット、work/2026-08-02-u8-task-attach あり)。
  docs/ は clean —— 移行は docs/ と README.md のみに触れる
- 非 md からの docs パス参照は spikes/p4-m1/run-m1-e2e.sh のコメント1行のみ(参照先は残置ファイル。影響なし)

## リンク修正のパターン(調査エージェントの被参照マップから)

- 残置(design/ の 00-final・01-decision-log 等)への参照 → `../design/<name>`
- 共移動ファイル同士 → bare 名(旧 `../pX-*.md`・`design/XX-*.md` 形式を解消)
- 旧 docs/ 直下発の bare `p4-findings.md`(残置)→ `../p4-findings.md`
- 旧 docs/ 直下発の `../spikes/` → `../../spikes/`(深さ+1)
- 無傷(修正しない): 旧 design/ 発の `../../spikes/`・`../p4-findings.md`、共移動同士の bare リンク
- L 側の修正対象5本: README.md(5リンク)/ docs/runbook.md(3)/ 00-final-design-plan.md(briefs・review・synthesis・06 への全出現)/ 01-decision-log.md(markdown リンクのみ最小修正)/ design/README.md(13リンク)
