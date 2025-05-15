# Obsidian Git v2.33.0 設定メモ

## 📅 目的

Obsidian Gitプラグインを用いて、Vaultの自動コミット、GitHubへのPush/プルを自動化するための設定をまとめました。

## ☑️ 設定実装時のマッピング (v2.33.0基準)

|目的|現設定ラベル|説明|
|---|---|---|
|自動コミット間隔|`Auto commit-and-sync interval (minutes)`|X分ごとにコミットする|
|編集完了時にコミット+Push|`Auto commit-and-sync after stopping file edits` + `Push on commit-and-sync`|作業完了時に自動Push|
|Obsidian起動時Pull|`Pull on startup`|Obsidian起動時にPullする|
|Pull on commit-and-sync|`Pull on commit-and-sync`|Push前にPullするかどうか|
|Push on commit-and-sync|`Push on commit-and-sync`|コミット後にPushする|
|自動コミットメッセージ|`Commit message on auto commit-and-sync`|例: `vault backup: {{date}}`|

---

## ▶️ おすすめの設定パターン

```text
◆ Auto commit-and-sync interval: 0
◆ Auto commit-and-sync after stopping file edits: ON
◆ Push on commit-and-sync: ON
◆ Pull on commit-and-sync: ON
◆ Pull on startup: ON
◆ Commit message: vault backup: {{date}}
```

---

## ✅ 動作メモ

- 編集完了時に自動でCommit & Pushされる
    
- 起動時にPullされるのでマルチデバイスでの使用に向いている
    

---

## ※ 補説

- v2.33.0 以降、自動コミットは `commit-and-sync` に集約
    
- Pushの最方にある `Push on commit-and-sync` をONにしないと自動Pushは動作しない
    
- Pullも同様に `Pull on commit-and-sync` をONにする必要があり
    

---

## 関連ノート


## 出典（あれば）
- 書籍・論文・記事などの引用元
