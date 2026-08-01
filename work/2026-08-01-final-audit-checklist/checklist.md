# 終了時監査(2026-08-08)統合チェックリスト

日付: 2026-08-01 / 位置づけ: C(work/。権威なし)

policy.md §12-2 の手順と、review.md §11 実施記録・improvement-plan I7 に分散した注意事項の集約。
リマインダ routine(`trig_01JyTpK1wHJnSMyC7zLj3Dhr`。2026-08-08 09:00 JST 発火)の手順書は
2026-07-30 時点のもので、**改名(2026-08-01)後の追加作業(§4 末尾)を含まない** —— 本書が最新。

## 0. 前提

- 実行主体: **ユーザーの承認のもとエージェントが実行**(§12-2)。定期スキャンではなく1回だけ
- 観測期間: 2026-07-25(v4 初回配布 = metsuke `af31b45`)〜 2026-08-08(§12-3)
- 判定は3値(§12-5): **着地する / 着地しない / 新規作業が発生せず観測できない**。
  3つ目は「成功」と読まない → 観測期間を2週間延長。延長しても新規作業が無ければ対象リポジトリの追加を検討

## 1. git 監査(metsuke で実行。コマンドは policy.md §12-2 から)

- [ ] tracked: `git log --since=2026-07-25 --diff-filter=A --name-only --pretty=format: -- '*.md' | sort -u | grep -v '^work/'`
- [ ] untracked: `git status --porcelain -uall | grep '^?? '` で `work/` 外の md を確認(`-uall` 必須)
- [ ] 形態2 の検出: `git log -M --diff-filter=R --since=2026-07-25 --name-status -- '*.md'` で `work/` からの移動を拾う
- [ ] 複製の照合: `grep -rl "artifact-policy v" ~/.claude/commands ~/.claude/CLAUDE.md ~/.codex/AGENTS.md ~/github/*/AGENTS.md 2>/dev/null` で複製を発見し、各コピーと §11 ブロックを **`diff` で内容照合**(鮮度は内容の一致で判定。版番号の不一致は古さを意味しない。違反はコピー版 > 本体版の方向のみ)
- [ ] 補助計装: `~/.claude/artifact-policy-observation/md-writes.jsonl` のクロス集計(スニペット未配布リポジトリが対照群)

## 2. 分類と判定

- [ ] `work/` 外の md は各ファイルについて **§5.1 の参照文の有無で昇格 / 流出に分類**してから判定に使う
- [ ] 観測項目: (i) C 性の成果物が `work/` に着地するか (ii) 昇格が実行されるか (iii) 形態2 が発生するか
- [ ] `metsuke: docs/claude-code-cost-workshop.md` の L/C を内容で確定(中間観測の流出候補。work/ 不経由・`fbcfbb7` 07-25 23:51。07-27 に事実修正を受け維持されており L 解釈の余地)
- [ ] 形態1 昇格(`work/2026-07-27-auto-mode-cost-attribution/findings.md` → docs 6ファイル)が `/wrap` 起点だったかをユーザーに確認(計装の効果測定)

## 3. 除外・特記事項(review.md §11 実施記録より)

- [ ] `metsuke: docs/08-dashboard-fb.md` へのマーカー付与(`2ae04ed`)は artifact-policy の管理作業 —— **流出・昇格のいずれにも数えない**。md-writes.jsonl の metsuke/outside 1件をクロス集計から除外
- [ ] `2ae04ed` の master マージ確認(2026-08-01 時点で topic ブランチ `docs/auto-gate-denial-observed` に滞留中。未マージなら master 側の同ファイルはマーカー無し = §2.1 で L 判定のまま)
- [ ] 本リポジトリ(agentic-coding-policy)は AGENTS.md 配布済み → **対照群集計から除外**
- [ ] `metsuke: work/2026-07-27-workshop-member-metrics/` の commit 状態確認(`3d6fb82` 07-31 で追跡済み。以後の新規分が untracked のままでないか)
- [ ] `metsuke: docs/cowork/0001-workshop-prep-002/intent.md` の完了時マーカー(タスク完了が確認できれば §3.3 を適用)
- [ ] **Codex・人手の書き込みは補助計装に映らない**限界を監査記録に明記する

## 4. 監査後の第7版バッチ(前倒し不可。監査と同日に実施)

第7版の**候補**(review.md §11。確定ではない —— 観測結果と突き合わせて第6巡として裁定する):
射程フィルタ(§2/§3)/ §5.5 優先規則(知見の有無を優先)/ 並列採番・容器新設は親のみ /
複数リポジトリ跨ぎのポインタ / CHANGELOG の一律 A 指定の限定 / 非文書成果物の昇格経路 /
slug 語彙の再利用 / §7.1 第4条件(自分以外の書き込み)/ 版履歴・単発リマインダの扱い

確定分:

- [ ] 終了時監査の結果を **review.md 第6巡**として記録し、§12-5 の3値判定を確定する
- [ ] 複製の全数更新(§12-8): §11 スニペット → v7 / `metsuke/AGENTS.md` / `~/.claude/commands/wrap.md`。
  自リポジトリ AGENTS.md は版ヘッダを持たない参照設計(第5巡-9)のため、§8 記述と食い違わない限り更新不要
- [ ] **改名の残存参照4箇所を新名に更新**(2026-08-01 追加。review.md §11 実施記録の裁定):
  policy.md §11 スニペットヘッダ / `metsuke/AGENTS.md:1` / `wrap.md:5・7` / `artifact-policy-log.sh:2`
- [ ] 本体ポインタの **URL 化を検討**: `github.com/taku7777777/agentic-coding-policy`
  (owner 無しの名前ポインタは第三者が辿れず、改名で腐ることが今回実証された)
- [ ] 展開判断(§12-7: グローバル1枚が第一候補)と D3(`~/.claude/CLAUDE.md` への §8 1行)の再判断
- [ ] D2 リマインド: LICENSE 未決のまま(第一候補 CC BY 4.0。ユーザー判断)
