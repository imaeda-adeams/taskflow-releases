# taskflow — Releases

開発者の作業記憶を外部化するタスク管理 CLI(配布専用リポジトリ)。

GitHub / GitLab の Issue を正としてローカルに取り込み、タスク切替時のコンテキスト
(メモ・次にやること・ブランチ・stash)を保存・復元します。

## インストール

[Releases](https://github.com/imaeda-adeams/taskflow-releases/releases) から OS 別バイナリをダウンロードし、
`taskflow` にリネームして PATH の通った場所に配置してください。

| OS | ファイル |
|----|---------|
| macOS (Apple Silicon) | `taskflow-aarch64-apple-darwin` |
| Windows (x64) | `taskflow-x86_64-pc-windows-msvc.exe` |
| Linux (x64) | `taskflow-x86_64-unknown-linux-gnu` |

macOS / Linux では実行権限を付与してください: `chmod +x taskflow`

### Windows: scoop でのインストール(推奨)

[scoop](https://scoop.sh) 経由ならブラウザの警告なしで導入・更新できます。

```powershell
scoop bucket add taskflow https://github.com/imaeda-adeams/taskflow-releases
scoop install taskflow
# 更新: scoop update taskflow
```

### ダウンロード時の警告について

バイナリはコード署名をしていないため、ブラウザや Windows SmartScreen が
「信頼できることを確認してください」等の警告を表示します。これは未署名バイナリに対する標準の警告です。

- ブラウザ: 「保持する」を選択 → 実行時は「詳細情報」→「実行」
- または PowerShell で: `Unblock-File .\taskflow-x86_64-pc-windows-msvc.exe`

改ざんされていないことは、各リリースに添付の `SHA256SUMS` で検証できます。

```powershell
# Windows
certutil -hashfile taskflow-x86_64-pc-windows-msvc.exe SHA256
# macOS / Linux
shasum -a 256 taskflow-aarch64-apple-darwin
```


### 依存コマンド(任意)

- `git` — ブランチ連携に必須
- `gh` 2.x 以上 — GitHub Issue 同期(`gh auth login`)
- `glab` 1.x 以上 — GitLab Issue 同期(`glab auth login`。セルフホストは `--hostname` 指定)

## 主なコマンド

```
taskflow add "<タイトル>"        # タスク作成
taskflow sync                    # Issue 取込(GitHub / GitLab 自動判別)
taskflow start <ID>              # 作業開始 + ブランチ作成/切替
taskflow note "<メモ>"           # 作業メモ
taskflow switch <ID> [--stash]   # 割り込みへ切替(退避)
taskflow resume [<ID>]           # 復帰(<プロジェクト>:<ID> で横断復帰)
taskflow done [<ID>]             # 完了(Issue 由来は確認後クローズ)
taskflow list --global           # 全プロジェクト横断表示
taskflow status --global         # 全プロジェクトの状況俯瞰
```

タスクデータはリポジトリ内 `.taskflow/` のプレーンテキストに保存され、外部送信されません。
認証トークンは保持せず、`gh` / `glab` に委譲します。

※ 本リポジトリはバイナリ配布専用です(ソースは非公開)。
