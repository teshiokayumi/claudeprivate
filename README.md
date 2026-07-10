# 🎵 Suno Music Player

Suno で制作した楽曲をブラウザでループ再生できるミュージックプレイヤーです。
GitHub Pages でそのまま公開でき、ポートフォリオからリンクして聴いてもらえます。

## 機能

- 🔁 全曲ループ / 1曲リピート / リピートなし の切り替え（デフォルトは全曲ループ）
- 🔀 シャッフル再生
- ⏯ シークバー・音量調整・キーボード操作（Space で再生/停止、Shift+←→ で曲送り）
- 📱 レスポンシブ対応、Media Session API 対応（スマホのロック画面から操作可能）
- ➕ `songs.json` にリンクを追加して push するだけで曲を追加できる

## 公開方法（GitHub Pages）

1. このブランチを `main` にマージ（または `main` に直接この内容を置く）
2. リポジトリの **Settings → Pages** を開く
3. **Source: Deploy from a branch**、**Branch: `main` / `(root)`** を選んで Save
4. 数分後に `https://<ユーザー名>.github.io/<リポジトリ名>/` で公開されます

> **注意:** GitHub Pages はプライベートリポジトリでは無料プランで使えません。
> その場合は、公開用に新しいパブリックリポジトリを作ってこのファイル一式を置いてください。

## 曲の追加方法

1. Suno で曲の共有リンクをコピー（`https://suno.com/s/...` 形式）
2. `songs.json` の `songs` 配列に 1 行追加:

```json
{ "url": "https://suno.com/s/XXXXXXXXXXXX" }
```

3. コミットして push すると、GitHub Actions（`Update playlist`）が自動で
   曲名・ジャケット画像・音声 URL を取得して `playlist.json` を更新します
4. GitHub の **Actions** タブでワークフローの完了を確認（1〜2分）

タイトルを自分で指定したい場合は `"title": "曲名"` を併記できます:

```json
{ "url": "https://suno.com/s/XXXXXXXXXXXX", "title": "お気に入りの曲" }
```

### 自動取得がうまくいかない場合（手動追加）

Suno 側のアクセス制限でワークフローが失敗した場合は、手動で追加できます。

1. ブラウザで共有リンク（`https://suno.com/s/...`）を開くと
   `https://suno.com/song/<UUID>` にリダイレクトされます
2. その UUID を使って `songs.json` に直接書きます:

```json
{
  "url": "https://suno.com/s/XXXXXXXXXXXX",
  "id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "title": "曲名"
}
```

`id` があるページ取得なしで `https://cdn1.suno.ai/<UUID>.mp3` を再生します。

## ファイル構成

| ファイル | 役割 |
|---|---|
| `index.html` | プレイヤー本体（依存ライブラリなしの単一ファイル） |
| `songs.json` | **編集するのはここ** — 曲リストの元データ |
| `playlist.json` | 自動生成される再生用データ（直接編集しない） |
| `scripts/resolve-playlist.mjs` | Suno リンクを解決するスクリプト |
| `.github/workflows/update-playlist.yml` | songs.json 変更時に playlist.json を自動更新 |

## ローカルで確認する

```bash
npx serve .
# または
python3 -m http.server 8000
```

ブラウザで `http://localhost:8000` を開きます（`file://` 直接開きでは playlist.json の読み込みがブロックされます）。

---

Music generated with [Suno](https://suno.com)
