# CLAUDE.md - neuralnaut docs

## このリポジトリ

https://docs.neuralnaut.com の中身。neuralnaut のサービスの規約類を置いている。

```
whisper/
├── index.md     # ささやきチャットの目次
├── terms.md     # 利用規約
├── privacy.md   # プライバシーポリシー
└── tokutei.md   # 特定商取引法に基づく表記
```

アプリ（cosmos の `login_page.dart` / `setting_page.dart`）が `https://docs.neuralnaut.com/whisper/{terms,privacy,tokutei}.html` にリンクしている。
**ファイル名を変えるとアプリ内のリンクが切れる**（アプリの審査を通さないと直せない）。

## 公開の仕組み

- GitHub Pages の legacy ビルド（Jekyll）。`main` ブランチの直下をそのまま公開する。CI は無い
- **public リポジトリで、`main` に push した時点で公開される**。下書きは `main` に入れない（ブランチで作って PR）
- `.md` は `.html` として公開される。公開したくないファイル（この CLAUDE.md など）は `_config.yml` の `exclude` に足す
- ドメインは `CNAME`（`docs.neuralnaut.com`）

## 書くときの注意

- 規約・ポリシーは**実装と申告に合わせる**。書いてあることと実際の挙動が違うと、ストア審査やユーザーとの間で問題になる。
  特にプライバシーポリシーは、App Store の「App のプライバシー」・Play のデータセーフティ・coreapi の保持と削除の実装と矛盾させない
- 改定したら各文書の「最終改定日」を更新する
- 実装の事実（保持期間・収集項目）は推測で書かない。coreapi のコードか、NEURA の記憶で確かめてから書く

## 進捗と記憶（NEURA MCP）

- プロジェクト: **`whisper`**（ささやきチャット）
- このリポジトリ: **`repo: "docs"`** — docs.neuralnaut.com（規約・プライバシーポリシー）

`whisper` には複数のリポジトリが関わる。**プロジェクトは共通、作業はリポジトリ単位**で扱う。

| ツール | project | repo |
|---|---|---|
| `create_task` / `list_tasks` | 必須 | **必須**（どのリポジトリの作業か分からないと着手できない） |
| `memory_set` | 必須 | 付ける（後で絞り込めるように） |
| `memory_search` | 必須 | **付けない**（他リポジトリの知見が効くことが多い） |
| `create_schedule` | 必須 | 付ける |

タイトルに `docs:` のような接頭辞は**付けない**（`repo` で区別できる）。
リポジトリ横断の設計判断・調整だけ `repo` を省略する。

### 作業の始めと終わり
- **始める前**: `memory_search`（`project: "whisper"`、**repoは指定しない**）で関連する記憶を引く。
  ポリシーの改定なら、計測・課金・退会まわりの `decision-` / `howto-` を必ず読む
- **始める前**: `list_tasks`（`project: "whisper"`, `repo: "docs"`）で自分の担当分を確認し、
  着手するものを `move_task` で `in_progress` に
- **終わるとき**: `move_task` で `done` に。`progress-<話題>` を上書きして状態を残す

### 記憶の種別
`decision-` （決定と**理由**） / `howto-` （手順） / `gotcha-` （罠） / `progress-` （進行状況）

**日付はキーに入れない**（同じ話題が日ごとに分裂して検索が効かなくなる）。
同じ話題は同じキーを上書きし、時系列は本文に書く。

### 書く前に
`memory_search` で似た記憶が無いか確認する。あれば新規作成せず同じキーを上書きする。

### 書かないこと
コードを読めば分かること / git履歴やチケットにあること / その場限りの試行錯誤

Web UI: https://neura.n2t.app/projects/whisper?repo=docs
