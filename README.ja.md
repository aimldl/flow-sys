Translations: [한국어](README.md) | [English](README.en.md) | **日本語**

# Flow OS

> 優先タスクへの復帰にかかる認知的負荷を最小限に抑え、フロー状態（没入）を促すシェルシステム。

## クイックスタート

```bash
git clone git@github.com:aimldl/flow-os.git ~/.flow-os
cd ~/.flow-os && chmod +x install.sh && ./install.sh
```

**シェルの再起動**

Bashをご利用の場合 (Linuxなど)
```bash
bash
```

Z Shellをご利用の場合 (macOSなど)

```bash
exec zsh
```
完全なインストールガイドは [INSTALL.md](INSTALL.md) を参照してください。

## Flow OSとは？

Flow OSは `~/.flow-os` に配置されるシェル設定レイヤーです。常に最後に作業していた場所を表示することで、作業開始の心理的摩擦を軽減します。記憶せず、迷わず、すぐに実行する。

```
[ 認知負荷の軽減 ] → [ 摩擦のない復帰 ] → [ 即座の再開 ]
```

## 仕組み

ターミナルを開くと、TODOリストと再開パスがダッシュボードに表示されます。`go` 一つで最後の作業ディレクトリに即座に移動します。

```
ターミナル起動 → Dashboard → go → 作業 → clean → quit
```

## 構造

```
~/
├── .flow-os/          # OSレイヤー（このリポジトリ）
└── my/                # データレイヤー
    ├── .flow/         # 状態レイヤー
    │   ├── resume.md
    │   ├── todo.md
    │   ├── memo.md
    │   ├── next.md
    │   └── v-*.md     # 月次アーカイブ（vは完了状態を意味する）
    ├── craft/         # 意図レイヤー
    ├── forge/
    ├── quests/
    ├── temp/
    └── vault/
```

## 主要コマンド

| コマンド | 説明 |
|----------|------|
| `go` | resume.mdに記録されたパスに移動 |
| `fmemo <テキスト>` | 雑念をmemo.mdに即座に記録 |
| `q` | git push → resume保存 → レビュー → ターミナル終了 |
| `qq` | git push → resume保存 → 即時終了（レビュー省略） |
| `cleanmemo` | memo.mdをhistoryにアーカイブ |
| `cleantodo` | 完了項目をhistoryに移動、未完了項目を維持 |

## ドキュメント

| ドキュメント | 説明 |
|-------------|------|
| [docs/workflow.md](docs/workflow.md) | 日常ワークフローガイド |
| [docs/commands.md](docs/commands.md) | 全コマンドリファレンス |
| [docs/philosophy.md](docs/philosophy.md) | 設計哲学と脳科学 |
| [docs/git-sync.md](docs/git-sync.md) | Git自動化設定 |
| [docs/memo-design.md](docs/memo-design.md) | メモシステム設計 |
