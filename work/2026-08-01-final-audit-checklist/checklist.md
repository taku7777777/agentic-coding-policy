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
- [ ] マーカー付与コミットの本流到達確認 —— **2系統に滞留**(2026-08-01 実測): `2ae04ed` は**マージ済み**ブランチ `docs/auto-gate-denial-observed` 上で合流経路なし / 同一パッチの `d078460` は `feat/cc-cost-peek-share-card` 系列(上に実作業が積まれており、ブランチのマージで自然到達する)。監査時点で origin/master 未到達なら、v7 移行(records/ への git mv)の**前に**処置を確定する(design.md §6-1。先に mv すると rename/modify 衝突で旧パスが復活しうる)
- [ ] 第6巡の記録に注記: review.md §11 実施記録(07-30)の「作業中の topic ブランチ」は記載時点で既に不正確だった(ブランチは 07-27 マージ済み。一次資料照合レビューの発見5)
- [ ] 本リポジトリ(agentic-coding-policy)は AGENTS.md 配布済み → **対照群集計から除外**
- [ ] `metsuke: work/2026-07-27-workshop-member-metrics/` の commit 状態確認(`3d6fb82` 07-31 で追跡済み。以後の新規分が untracked のままでないか)
- [ ] `metsuke: docs/cowork/0001-workshop-prep-002/intent.md` の完了時マーカー(タスク完了が確認できれば §3.3 を適用)
- [ ] **Codex・人手の書き込みは補助計装に映らない**限界を監査記録に明記する

## 4. 監査後の第7版バッチ(前倒し不可。監査と同日に実施)

**v7 の本体は `work/2026-08-01-records-redesign/design.md`**(L/A 判別のパス一本化。2026-08-01 に
独立レビュー3本で裁定済み。未裁定2点は同 §11)。旧候補のうち records 設計が吸収・解消するもの
(射程フィルタ / 並列採番 / CHANGELOG / 非文書昇格 / §5.6)は同設計 §7 の表に従う。
**影響を受けない候補は従来どおり第6巡で裁定する**(黙って落とさない):
§5.5 優先規則(知見の有無を優先 —— 候補中の最有力)/ 複数リポジトリ跨ぎのポインタ /
slug 語彙の再利用 / §7.1 第4条件(自分以外の書き込み)/ 版履歴・単発リマインダの扱い

確定分:

- [ ] 終了時監査の結果を **review.md 第6巡**として記録し、§12-5 の3値判定を確定する
- [ ] 複製の全数更新(§12-8): §11 スニペット → v7 / `metsuke/AGENTS.md` / `~/.claude/commands/wrap.md` /
  **自リポジトリ AGENTS.md(全面改稿 —— 現行はマーカー方式・直下 A を規定しており records 設計と確実に食い違う。2026-08-01 レビュー M6)**
- [ ] **改名の残存参照4箇所を新名に更新**(2026-08-01 追加。review.md §11 実施記録の裁定):
  policy.md §11 スニペットヘッダ / `metsuke/AGENTS.md:1` / `wrap.md:5・7` / `artifact-policy-log.sh:2`
- [ ] 本体ポインタの **URL 化を検討**: `github.com/taku7777777/agentic-coding-policy`
  (owner 無しの名前ポインタは第三者が辿れず、改名で腐ることが今回実証された)
- [ ] 展開判断(§12-7: グローバル1枚が第一候補)と D3(`~/.claude/CLAUDE.md` への §8 1行)の再判断
- [x] D2: 解決済み —— LICENSE は CC BY 4.0 で確定・配置(2026-08-01。review.md §11 実施記録)
