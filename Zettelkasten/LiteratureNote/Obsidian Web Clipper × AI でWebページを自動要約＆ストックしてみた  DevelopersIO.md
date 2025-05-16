---
title: "Obsidian Web Clipper × AI でWebページを自動要約＆ストックしてみた"
source: "https://dev.classmethod.jp/articles/obsidian-web-clipper-ai-summary/"
author: "豊島"
created: 2025-05-16
tags:
  - "obsidian"
  - "web-clipper"
  - "ai"
  - "productivity"
  - "tools"
---

こんにちは、豊島です。

最近、ノート系のアプリケーション（NotionやInkdropなど）を片っ端から試しており、現在はObsidianを愛用しています

Obsidianは豊富なプラグインがあることで有名ですが、今回はブラウザの拡張機能 Obsidian Web Clipper を使ったWebページ要約のTipsを紹介します

- Obsidianを使っている方（使ってみようと思っている方）
- ブラウザのタブが「後で読みたい記事」で溢れている方

におすすめです

### インタープリターの設定方法

拡張機能からObsidian Web Clipperの設定画面を開き  
サイドバーの `インタープリター` から、以下の設定を行います

1. プロバイダーを指定  
	Google Gemini, DeepSeek, OpenAIなどが選択できます  
	![Arc-2025-03-05 at 21.11.49](https://devio2024-media.developers.io/image/upload/v1741179065/2025/03/05/r7rrozbemuavqvv19dbj.png)  
	プロバイダーを指定する際にAPIキーを入力します  
	![Arc-2025-03-05 at 21.13.20](https://devio2024-media.developers.io/image/upload/v1741179061/2025/03/05/qdodmkpxxme7lzvicgvh.png)
2. モデルを選択  
	今回はGemini 2.0 Flashを選択しました
3. 詳細設定  
	`{{fullHtml}}` にしておきましょう

### テンプレートの設定方法

サイドバーの `テンプレート` から、ノートの保存先ディレクトリを指定し、 `ノートの内容` にプロンプトを記載します  
今回は下記のように設定してみました  
![Arc-2025-03-05 at 21.40.52](https://devio2024-media.developers.io/image/upload/v1741179044/2025/03/05/dzropnspzzlhkrtn68ox.png)

```
{{"内容を簡潔に要約してください。また要点ごとにマークダウン形式で簡潔にまとめてください。その際、箇条書きや見出しを活用し重要なポイントが一目で分かるようにしてください。"}}
```

これで最低限の設定は完了です

### Webページの要約を試してみる

以下の記事を例に試してみます

拡張機能からObsidian Web Clipperを開くと、自動で要約が開始します

要約開始  
![Arc-2025-03-05 at 21.26.16](https://devio2024-media.developers.io/image/upload/v1741179057/2025/03/05/fpmekbt7puepmcmj3vys.png)

要約完了  
![Arc-2025-03-05 at 21.26.26](https://devio2024-media.developers.io/image/upload/v1741179054/2025/03/05/gszg6msfuljfbgfpeznw.png)

今回は約2.5秒で要約が完了しました

要約内容も簡潔で分かりやすくまとまっています  
![Obsidian-2025-03-05 at 21.28.21](https://devio2024-media.developers.io/image/upload/v1741179048/2025/03/05/pwpx7ipsfdoenvo1fpgk.png)

本文を読みたくなった場合は、プロパティ内の source リンクから元の記事にアクセスできます

## 最後に

最近は気になる記事を見つけたらひとまずこの方法で保存し、時間のあるときに見返してObsidian内で整理しています  
この記事がどなたかの参考になれば幸いです