63

4

[![](https://static.zenn.studio/images/drawing/idea-icon.svg)](https://zenn.dev/tech-or-idea)

[idea](https://zenn.dev/tech-or-idea)

## はじめに

前回の [OHZフロー](https://zenn.dev/estra/articles/ohzflow-zenn-hugo-obsidian) についての記事から更に応用的運用を行うためのフローについての Book につながる内容です(現在作成中)。

今回は Obsidian 内部の **ナレッジベースからAnkiへとフラッシュカード化する方法** とそのプラグインを紹介します。前回は OHZ フロー(Zenn & Hugo in Obsidian)だったので、今回も暫定的に **OTA(Obsidian To Anki)フロー** と呼んで名称の簡略化と概念化を行います。

Obsidian とこの記事で紹介するプラグインの両方とも開発の中途段階なので今後様々な変更があると思われます。注意してください。

現在の環境は以下です。  
Obsidian: insider v0.10.4  
Obsidian\_to\_Anki: v3.4.1

また著者も研究途中の場所があるのでその部分には触れていません。今後この記事を更新する際に順次追加する予定です(OHZ フローで Obsidian 内部の保管庫から記事を publish しているので更新頻度は高いです)。

「そもそも Obsidian て何よ？」っていう人は [前回の記事](https://zenn.dev/estra/articles/ohzflow-zenn-hugo-obsidian) を見てみてください。

## 対象読者

- [Obsidian](https://obsidian.md/) と [Anki](https://apps.ankiweb.net/) に興味がある人
- SRS([Spaced Repetition System](https://docs.ankiweb.net/#/background))に興味がある人
- ナレッジベースから効率的にフラッシュカードを作成したい人
- 長期的に記憶したい事がある人
- 情報を分散させたくない人
- **問題解決と学習のシームレスな統合** を行いたい人

## 注意点

Obsidian\_to\_Anki v3.4.1 で起きうる可能性のある問題を「現時点での課題」として追記しました。 **メインの保管庫で本格運用を行う前にテストを十分に行ってから** 実践するようにしてください。

また、wiki そのものをまとめているこの記事自体の量がそこそこ多いです。Anki を触れたことがないとちょっと厳しいので、まずは Anki で遊んでみることからはじめて欲しいと思います。Anki ユーザーに有名な次のサイトがおすすめです。

Anki 自体もそうですが、紹介するプラグイン自体がかなりクセがあるので、たぶんこの記事を最初見た人たちの何人かは確実に躓くと思います。なので記事自体は読んでほしいですが、急ぎの方は「Zettelkasten と SRS」の項目だけ読んでもらって、あとは直接触らずに時間があるときに色々実験してみてください。

プラグインについての質問などあればコメント欄にしてください。

---

## ZettelkastenとSRS

今回は Obsidian(方法論: Zettelkasten)と Anki(方法論かつアルゴリズム: SRS)を組み合わせるためのプラグインの使用方法とその運用について説明していくが、そもそも Zettelkasten と SRS 自体があまり知られてないので簡単に説明しておく。

ちなみに前回の記事で紹介した LYT(**Linking Your Thinking**)は Zettelkasten をデジタル環境であつかいやすくするような応用的方法論なので分かりづらくなるかもしれないが注意してほしい。コア概念は Zettelkasten と SRS だ。

## SRSとは?

**Spaced Repetition System** の略。 **情報を復習するための理想的な時間を追跡し､ユーザーの記憶パフォーマンスに基づいて､その時間を最適化する** というシステム。Anki は SRS を実装したソフトウェアであり、世界中の人々から愛されている。

簡単にイメージしてもらうと [エビングハウスの忘却曲線](https://atsueigo.com/forgettingcurve/) を情報ごとに(例えば、英単語 5000 個それぞれの復習時間を)管理し、テスト(**Active Recall Testing**)によってその曲線を情報ごとに最適化していくようなシステムのこと。色々なアルゴリズムがあるが Anki は SM-2 アルゴリズムを採用している。ちなみに Anki は、1972 年ごろに提唱された概念「Spaced Repetition」をソフトウェアで実用化した SuperMemo にインスパイアされて開発されたユーザーフレンドリーなソフトウェアの名称である(名称は日本語の暗記から来ている)。

> SRSによって､情報の復習間隔をばらけさせることで3000枚ものフラッシュカードでもコントロール可能になる｡それぞれ **いつ復習すべきは､SRSが情報ごとに追跡して復習間隔を最適化してくれる** からだ｡人間はそれをいちいち考える必要性はない､むしろ大量の情報を分散的に追跡するのは人力では困難である｡使用しなければ忘れるという人間の記憶に対して､SRSというソフトウェアのシステムによって **情報をそれぞれ追跡管理することで､大量の情報に対して Active Recall Testing を可能とする方法** を実現した｡  
> \- 自分のブログ [まずAnkiより始めよ](https://www.ankiyorihajimeyo.com/anki/mazuankiyorihajimeyo/) より引用

こまかいことは Hugo のブログに載せてるのでそちらを参照してほしい。

余談になるが SRS の最先端になるとメディアそのものと SRS を組み合わせた「 [Mnemonic medium](https://notes.andymatuschak.org/z4rRX3qwSSJRsEkdXKwH2shamgHNeRthrMLiF?stackedNotes=z2fBHADWa93EZTuNzuww7V3Vi587ZyZ4FHTHm) (ニーモニックメディア)」なるものが開発されているとのこと。

## Zettelkastenとは?

Zettelkasten(独: **ツェッテルカステン** 、英: ゼトルカステン)とは第 3 世代システム理論「オートポイエーシス」を社会システム理論に応用したドイツの社会学者ニクラス・ルーマンが作り出したノートテイキング方法。具体的には、断片的なノート同士をリンクすることで、読んだり考えたことをすべて思い出せるようにし、更にはリンク(参照)によって浮かび上がってくる知識間の新たな関連性や思考のつながりを発見することを可能とする方法論である。 [^1]

ルーマンが考案した時代においてはキャビネットで大量のカード(ノート断片)を管理していたが、今やデジタルデバイスを一人一台もつようになり、様々な人々が Zettelkasten を実践するようになった。デバイス内部やインターネットなどのデジタル環境においてもハイパーリンクに始まり、双方向リンク、バックリンク、トランスクルージョン(埋め込み参照)技術などによってリンク自体も進化してきている。

Obsidian や Roam Research、Scrapbox、Notion などの新しいメモツールやネット技術そのものの進歩により Zettelkasten のさらなる進化が起きている。その一端が [Evergreen Note](https://publish.obsidian.md/andymatuschak/Evergreen+notes) や [LYTフレームワーク](https://publish.obsidian.md/lyt-kit/MOCs+Overview) などである。

## 二つのプラグイン

Obsidian 内部の保管庫から Anki 化(OTA フロー)するためのプラグインは現時点で二つある。

- [reuseman/flashcards-obsidian: An Anki plugin for Obsidian.md](https://github.com/reuseman/flashcards-obsidian)
- [Pseudonium/Obsidian\_to\_Anki: Script to add flashcards from text/markdown files to Anki](https://github.com/Pseudonium/Obsidian_to_Anki)

基本的には reuseman が開発した Flashcards-obsidian のプラグインの方が簡単で使いやすい。ファイルごとに Anki 化するコマンドを打てたり、タグを指定するだけで Anki 化できるので手軽に Anki 化したい人には reuseman の Flashcard がおすすめ。

ただ、ちゃんとやりたいタイプの人には Pseudonium の Obsidian\_to\_Anki がかなりおすすめ。こちらでは **個人の運用に最適化することが可能** 。しかしそれだけ **文法が多くなったり、設定が複雑になる** のでプラグインの使い方を覚えるのが大変。また運用戦略なども考える必要がでてきたり、現在は保管庫全体をスキャンして Anki 化するので人によってはデメリットなどが存在する可能性もあるので注意して使う必要がある。

シンプルな Anki カードやシンプルな運用が好きな人は reuseman の Flashcards-obsidian を使ってほしい(Flashcard でできることは基本的に Obsidian\_to\_Anki でも可能だが、Obsidian\_to\_Anki はファイル単位での同期がまだできない)。

今回は Obsidian\_to\_Anki の利用方法についてメインで解説していく。

## Obsidian\_to\_Ankiの特徴

\-->\[\[Obsidian Integration\]\]

OTA(Obsidian\_to\_Anki)のプラグインでは **Obsidian特有の機能を活かしつつAnki化する事ができる** 。以下にどのような統合が図られているか紹介する。

- **URIを利用したソースとなるファイルへのリンク** を自動生成することが可能
	- どのフィールドにリンクを作成するか設定から追加する。
- リンクのサポート
	- markdown 形式のリンク `[]()` と wikilink 形式のリンク `[[]]` をフルサポートしている。
	- リンクは URI リンクに変換され、 **クリックすることでObsidianのノートを開くことができる** 。
- 埋め込みのサポート -->\[\[Image formatting\]\]
	- wikilink の `![[]]` 形式と markdown の `![]()` 形式両方の音声と画像埋め込みをサポートしている。
	- **自動的に画像や音声がAnkiにコピーされる** 。
- タグのサポート
	- Obsidian の `#tag` を Anki のタグに変換可能。
- 目次によるコンテキストサポート
	- 目次(#)レベルの情報をカードに追記。
- フォルダごとの設定を追加可能
	- フォルダごとにどのタグを挿入するかを設定可能。
- マークダウンフォーマットのサポート
	- マークダウンのフォーマットは基本的にサポートされている。-->\[\[Markdown formatting\]\]

## OTA導入の準備

## チュートリアル動画

<iframe src="https://www.youtube-nocookie.com/embed/PXyv6pnVGhA" allow="accelerometer; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen=""></iframe>

Santi Younger による英語でのセットアップ説明動画。ノートタイプなどについては詳しい説明はないがチュートリアルとして分かりやすい。英語がわかる人はざっと見てみると概要が分かる。

今回はこの動画に書いてないような詳しいやり方について運用戦略と絡めて説明していく。

## プラグインのインストール

Anki と Obsidian がインストールされている状態を前提として話を進める。

必要なアドオン AnkiConnect と Obsidian\_to\_Anki をそれぞれ Anki と Obsidian にインストールする。

注意) Anki と Obsidian 共にテスト用のプロファイルとテスト用の保管庫でまずは実験してみることをおすすめする。

- [AnkiConnect 1. AnkiWeb](https://ankiweb.net/shared/info/2055492159)
- [Pseudonium/Obsidian\_to\_Anki: Script to add flashcards from text/markdown files to Anki](https://github.com/Pseudonium/Obsidian_to_Anki)

**Anki側**  
\-->\[\[Setup\]\]  
次の様に「ツール」→「アドオン」→「新たにアドオンを取得」→「アドオンをインストールする」まで行く。

![zennimage](https://storage.googleapis.com/zenn-user-upload/gwovmvagbzar7iczyiha4fji5olu)

![zennimage](https://storage.googleapis.com/zenn-user-upload/3azwik0efauypg3o3hevcrh8ugnz)

![zenn image](https://storage.googleapis.com/zenn-user-upload/8feaezdfy2udd1d1bs0mc3nqbi3k)

コードに `2055492159` をペーストして OK をおすと自動インストールされる。AnkiConnetct を選択して右下の「設定」をクリックして、最後のところに画像のようにコードをいれる。

```
"webCorsOriginList": [
    "http://localhost",
    "app://obsidian.md"
]
```

![zennimage](https://storage.googleapis.com/zenn-user-upload/vlpnf39blldwp4fk0xqgdbzmodqw)

アドオンと設定を有効化するために Anki を再起動し、そのままの状態にしておく。

**Obsidian側**  
「設定」→「サードパーティプラグイン」→「閲覧」→ Anki で検索すると Obsidian\_to\_Anki のプラグインが出てくるので「インストール」する。「インストールされたプラグイン」の項目から有効化する。

![zennimage](https://storage.googleapis.com/zenn-user-upload/viqtqc9ol13loqd13vxjhbnh99zx)

これで二つの準備が完了した。

## wikiをcloneする

OTA(Obsidian\_to\_Anki)プラグインの Wiki があるので、必ず参照してほしい。  
git を利用できる場合には OTA プラグインの wiki を自分のローカル環境に clone する。

[Home · Pseudonium/Obsidian\_to\_Anki Wiki](https://github.com/Pseudonium/Obsidian_to_Anki/wiki) にアクセスして  
`Clone this wiki locally` のボタンをクリックして URL をコピーする。

![zennimage](https://storage.googleapis.com/zenn-user-upload/y1vntt5b8rj5sdpxoquafcr0sdh6)

Termianal から適当なディレクトリに `git clone` する(Obsidian のテスト用の保管庫などに clone すると便利)。

```shell
$ git clone https://github.com/Pseudonium/Obsidian_to_Anki.wiki.git
```

![zennimage](https://storage.googleapis.com/zenn-user-upload/3w78ar4app32bmhoevfoc7m1930u)

\[\[Obsidian\_to\_Anki wiki MOC|wiki 用の MOC\]\]を作成して管理する。 基本的には wiki の\[\[Home\]\]からすべてのコンテンツにアクセスできる。

wiki のリポジトリが入っている保管庫でこのプラグインのテストすると wiki 内での説明に利用している文法によって **エラーが起きる** ので別のテスト保管庫を作成して同時起動し、そちらでプラグインのテストを行う必要がある。

clone しなくても web から見れるので git を利用しない場合には web の内容をそのまま参照してほしい。ちなみにこの記事を Wiki をクローンした保管庫にコピペするとそのまま Wiki 内の markdown ノートにリンクできるので活用してみてほしい(リンク切れなどあったらごめんなさい)。

![zennimage](https://storage.googleapis.com/zenn-user-upload/8z2pmd1vrjvvkzh0ts59zrqs0ad5)

---

(**ここからはAnkiの前提知識が必要となるので急ぎの方はブラウザバックしてください**)

## 実際の使い方

ここからは Anki についての基礎知識(デッキ、ノートタイプ、タグなどの用語が分かる)があることを前提としてすすめる。

## 2レベルの条件指定

まず知っていてほしいことは、タグやデッキに関しては、Anki 化するブロックごとに条件を指定する(1) **ノートベースのレベル** とファイル内のすべてのブロックに共通の条件を指定する(2) **ファイルベースのレベル** の二つが存在するということだ。

後で説明するが、ファイルベースの条件指定を利用すれば、いちいちすべてのブロックにデッキなどを指定する必要がなくなる。

## 基本的な文法

\-->\[\[Note formatting\]\]

では準備がととのったことで実際の使い方を紹介していく。

Anki 化するための一番易しい方法は、Obsidian\_to\_Anki の標準文法 `START/END` で Anki 化したい内容を囲み、コマンドパレットから `Obsidian_to_Anki: Scan Vault` を実行するかリボンにある Anki アイコンをクリックすることで自動的に保管庫全体をスキャンする(現時点ではファイルごとの Anki 化ができない)。

後述するが、カスタムシンタックスを利用すれば標準文法の `START/END` を利用せずに `== ==` でハイライトした部分だけを cloze 化するなどもできる。

### 標準文法 START/END

大文字の `START` と `END` で囲まれた部分が Anki 化される。最初の行にノートタイプを書き、次の行からノートのフィールド名とフィールドに記載する内容を書く(この `START` と `END` のキーワードは設定から変更することができる)。

文法

```markdown
START
{Note Type}
{Note Fields}
Tags:
END
```

具体例

```markdown
START
基本
Front: これはテスト
Back: テスト成功
Tags: テスト
END
```

一番最初のフィールドのプレフィックス(具体例だと Front フィールド)を省略することができる。  
\-->\[\[Field formatting\]\]

最初のフィールドを省略

```markdown
START
基本
これはテスト
Back: テスト成功
Tags: テスト
END
```

次の行にも続けることができる。

行を続ける

```markdown
START
基本
これはテスト
これもフロントフィールドに追加される
Back: テスト成功
Tags: テスト
END
```

**すべてのフィールドを書く必要がなく省略することができる** 。

Obsidian\_to\_Anki プラグインのコマンドを走らせると次のようにブロック内に一意の ID が挿入される。 -->\[\[Updating existing notes\]\]

Anki化後のIDが挿入される

```
START
基本
これはテスト
Back: テスト成功
Tags: テスト
<!--ID: 1566052191670-->
END
```

これによって、フィールドの内容を変更した後もコマンドを実行することで Anki のカードをアップデートすることが可能になる。

`Tags:` はタグを書かない限り、省略したほうが良い。

### ノートレベルの複数タグ指定

\-->\[\[Tag formatting\]\]

複数のタグを挿入したい場合には次のようにタグの間にスペースを入れる(`Tags:` の後にもスペースが必要)。

```markdown
START
基本
これはテスト
Back: テスト成功
Tags: Tag1 Tag2 Tag3
<!--ID: 1566052191670-->
END
```

### デフォルトノートタイプ

設定項目においてデフォルトとなるノートタイプは存在しない。つまり、 `START/END` の標準文法で囲っても、 **ノートタイプを正しく指定しない限りAnki化されない** 。

『Anki 側に「基本」というデッキが存在するとして』

具体例1

```markdown
START
基本
Front: これはテスト
Back: テスト成功
Tags: テスト
END
```

↑これは Anki 化される。

具体例2

```markdown
START
Front: これはテスト
Back: テスト失敗
Tags: テスト
END
```

↑これは Anki 化されない。

具体例3

```markdown
START
きほん
Front: これはテスト
Back: テスト失敗
Tags: テスト
END
```

↑これも Anki 化されない(「きほん」というデッキが Anki に存在しない場合)。

Anki 化するには正しくデッキ名を最初に宣言する必要がある。

### インラインノート

\-->\[\[Inline notes\]\]  
インラインで Anki 化したい場合には次の  
`STARTI` と `ENDI` で囲む。

インラインの基本文法

```markdown
STARTI [{Note Type}] {Note Data} ENDI
```

具体例

```markdown
STARTI [Basic] これはテスト Back: テスト成功 ENDI
```

この場合には Basic ノートタイプで保存される。しかし、次のように失敗するパターンが何個かあるので気をつけてもらいたい。

失敗例1(ノートタイプを書かない)

```markdown
STARTI これはテスト Back: テスト失敗 ENDI
```

失敗例2(ノートタイプが存在しない)

```markdown
STARTI [BAsiC] これはテスト Back: テスト失敗 ENDI
```

この二つのパターンは `START/END` ブロックでノートタイプを正しく書かないと Anki 化されないのと同じように正常に Anki 化されない。

### Cloze フォーマット

\-->\[\[Cloze formatting\]\]  
様々なスタイルから cloze を作成することが可能。もちろん Anki 標準の cloze フォーマット `{{c1::クローズノート}}` をそのまま使うことも可能。

| Clozeスタイル | 変換後(Ankiカード) |
| --- | --- |
| これは{一つ目のcloze}になる。これは{二つ目のcloze}になる。 | これは{{c1::一つ目のcloze}}になる。これは{{c2::二つ目のcloze}}になる。 |
| これは{2:二つ目のcloze}になる。これは{1:一つ目のcloze}になる。 | これは{{c2::二つ目のcloze}}になる。これは{{c1::一つ目のcloze}}になる。 |
| これは{1\|一つ目のcloze}になる。これは{2\|二つ目のcloze}になる。 | これは{{c1::一つ目のcloze}}になる。これは{{c2::二つ目のcloze}}になる。 |
| これは{c1:一つ目のcloze}になる。これは{c2:二つ目のcloze}になる。 | これは{{c1::一つ目のcloze}}になる。これは{{c2::二つ目のcloze}}になる。 |
| これは{c1\|一つ目のcloze}になる。これは{c2\|二つ目のcloze}になる。 | これは{{c1::一つ目のcloze}}になる。これは{{c2::二つ目のcloze}}になる。 |

上のスタイルを混ぜて使用可能であり、id(番号)を指定しない場合には id 指定されてない最初の cloze が 1 番目として扱われ、順次カウントアップされるようになる。

具体例

```
idを指定しない場合の{複数}の{クローズ}では、{2:id指定したクローズ}と同じく、{c1|他のスタイル}と合わせて使うことができる。
```

↓Anki での変換後

```
idを指定しない場合の{{c1::複数}}の{{c2::クローズ}}では、{{c2::id指定したクローズ}}と同じく、{{c1::他のスタイル}}と合わせて使うことができる。
```

`{}` カーリーブラケットの内側に数式を書いても OK。

## ノートの削除

\-->\[\[Deleting notes\]\]

Anki へ送ったノートブロックの削除についてはとくに気を付ける必要がある。

Anki 化したブロックを削除したい場合には **直接そのブロックを削除してはいけない** 。削除対象のブロックから離れた場所に最低一つは行を入れて次のコードを書いた状態でプラグインのコマンドを走らせる必要がある。

```
DELETE
ID: 123456789
```

ID には各ブロックに生成された番号で削除したいブロックの番号を指定する。

複数のブロックを削除したい場合には次のようの `DELETE` ブロックを複数書く。

```
DELETE
ID: 123456789

DELETE
ID: 294014480
```

削除したいブロックを再度 Anki 化したい場合には次のようにブロック下部に挿入されている ID の行をすべて削除する必要がある。 `DELETE` を実行した ID のブロックについては内容そのものを削除しない限りプラグインを走らせても Anki 化されることはない。

```
The idea of {cloze paragraph style} is to be able to recognise any paragraphs that contain {cloze deletions}.
<!--ID: 1609492666924-->
```

## ファイルの名称変更

Obsidian 側のでソースとなるファイル(ノート)の名称を変更した場合には再び OTA のプラグインを走らせて更新する必要がある。自動でソースとなるノートの URI リンクを Anki 側に送る設定にしているとその URI リンクが切れてしまうからだ。

ファイル名を変更したら単純にプラグインを再度走らせるだけで Anki カードを更新してくれる(コンテキストと URI が更新されるのは確認済み)。

## シンタックスハイライト

Back などにコードスニペットを書けば、自動的にコードハイライトしたカードが作成される。

```md
\`\`\`ts
class Point{
    x: number;
    y: number;
    
    constructor(x: number, y: number){
        this.x = x;
        this.y = y;
    }
    add(point: Point) {
        return new Point(this.x + point.y + point.y);
    }
}
// プロパティ、コンストラクタ、メソッドの定義

var p1 = new Point(0, 10);
var p2 = new Point(10,20);
var p3 = p1.add(p2); //メソッドの使用
\`\`\`
```

こんな感じ↓

![OTA-code](https://storage.googleapis.com/zenn-user-upload/t35qf82a1tk0a0aqwzusvkaoxpey)

これとは別に Anki に Prism.js を入れる方法などもある。  
参考: [Ankiにprism.jsを入れる](https://www.ankiyorihajimeyo.com/anki/prism_codehighlight_into_anki/)

## ファイルベースの条件指定

ファイルベースの条件指定を利用すれば、いちいちすべてのブロックにタグやデッキなどを指定する必要がなくなる。これを組み合わせて使うのが標準的な使用方法になると思われる。

ちなみに Folder Settings でも同様のことができる(こちらはフォルダ単位)。

## タグ指定 FILE TAGS

\-->\[\[Tag formatting\]\]

`FILE TAGS` のキーワードと共にファイルのどこか(主にトップがボトムの箇所に)次のようなコードを置く。

パターン 1

```markdown
FILE TAGS
数学 学校 物理
```

パターン 2

```markdown
FILE TAGS: 数学 学校 物理
```

この二つのパターンのどちらかのコードをファイルのどこかに入れる。

## デッキ指定 TARGET DECK

\-->\[\[Deck formatting\]\]

タグと同じ用に `TARGET DECK` キーワードと共にファイルのどこかに次のようなコードを挿入する。Anki 化されるブロック内部や Note type Settings でデッキが指定されてない場合にこのブロックで指定したデッキに Anki 化されることになる。

パターン(1)

```markdown
TARGET DECK
数学デッキ
```

パターン(2)

```markdown
TARGET DECK: 数学デッキ
```

## フィールド凍結 FROZEN

\-->\[\[Frozen Fields\]\]

[AnkiのFrozen Fields](https://ankiweb.net/shared/info/516643804) プラグインからインスパイアされた機能。

対象となるファイルのどこかに次のようなコードを書く。指定したノートタイプのフィールドを凍結させる(つまり、そのノートタイプのすべての Anki カードのフィールドを同一の内容を書くようにする)。

文法

```markdown
FROZEN - {Note type}:
{Field 1}: {Content 1}
{Field 2}: {Content 2}
```

具体例

```markdown
FROZEN - English Vocabulary:
CreatedDay: 2021-01-03
Source: Toelf 3800
```

具体例のように書くとそのファイル内のあらゆるノートタイプ「English Vocabulary」となるノートタイプになるブロックにおいて、CreatedDay(作成日)のフィールドに「2021-01-03」が追加され、Source フィールドに「Toelf 3800」が追加される。

エラーが起きないように他のコンテンツから離れるように一行あける。またファイルの一番下かトップに置いておくように推奨されている。

## 上記三つの組み合わせ

具体例

```markdown
TARGET DECK
数学デッキ

FILE TAGS
数学 学校

FROZEN - Math:
Context: Linear algebra
```

上記三つのファイルベースの条件指定を組み合わせるとこのようになる。これはファイルのすべてのノートブロックを「数学デッキ」にし、「数学、 学校」タグを挿入した Anki カードを作成する。

ただし「Math」ノートタイプがある場合にはコンテキストフィールドに「Linear algebra」を挿入する。

タグ指定とデッキに指定は同様にファイルベースでの条件指定だが `FROZEN` だけ特殊なので気をつけてほしい。 `FRONZEN` は作成日などの同一情報を複数の Anki カードで同じフィールドに何回も書かなくてすむようにする機能だ。

## 設定画面

## Note Settings: Ankiのノートごとの設定

`Note Type Table` が存在する。

| Note Type | Custom Regexp | File Link Field | Context Field |
| --- | --- | --- | --- |
| ノートタイプを指定 | カスタムシンタックス用のRegexを指定 | ソースリンクを挿入するフィールドを指定 | コンテキストを挿入するフィールドを指定 |
| TestNoteForOBANKI | ((?:.+\\n) *(?:.*\==.*)(?:\\n(?:^.{1,3}$\|^.{4}(?<!<!--).*))\*) | SourceLink | Context |

\-->\[\[Regex\]\]

この設定項目から使いたい正規表現パターンを指定してあげる。例えば上の例で使っている `((?:.+\n)*(?:.*==.*)(?:\n(?:^.{1,3}$|^.{4}(?<!<!--).*))*)` のパターンを指定すると、ハイライトされた箇所があるブロックは強制的に TestNoteForOBANKI のノートタイプとして Anki 化されるようになる。

## Folder Settings: Obsidianのフォルダごとの設定

| Folder | Foder deck | Folder Tags |
| --- | --- | --- |
| フォルダ名 | フォルダ内のノートから生成されるAnkiカードのデッキを指定 | フォルダごとに挿入するタグを指定 |
| TestFolder | TestAnkiDeck | TestObsidian |

## 文法の設定

![zennimage](https://storage.googleapis.com/zenn-user-upload/c3mrwfpa93mcqb8ja36op55yqwwq)

上で説明したきた `START/END` などのキーワードによる文法そのものを変更することがで可能。

個人最適化することが可能だが、トラブルシューティングで困る可能性もあるので何も問題がなければこのままデフォルトの文法でいこう。

## 基本設定

![zennimage](https://storage.googleapis.com/zenn-user-upload/sgfuqfz15b0pt0b506iiwc1gcov2)

- Defaults Tag
	- 自動的に Anki 化する際に挿入するタグを決める。
- Deck
	- 何もしていされてない場合に送られる Anki のデッキを決める。
- Scheduling Interval
	- 保管庫を自動スキャンする際のインターバル。自動スキャンしない場合には 0 に設定して毎回手動でスキャンする。
- Add File Link
	- ソースとなる Obsidian のノートへの URI リンクを追加するかどうか。
	- オンにする場合には URI リンクを含めるフィールドを Note Type Setting の File Link Field で指定する。
- Add Context
	- ファイル名と目次のレベルをコンテキストとして追加するかどうか。
	- オンにする場合にはコンテキストを含めるフィールドを Note Type Setting の Context Field で指定する。
- CurlyCloze
	- カーリーブラケット `{}` を Cloze 化するかどうか。
	- 利用する場合にはオンにした上でノートタイプを決めてあげて Note Type Settings の Custom Regexp に正規表現パターンをコピペしてあげる。
- CurlyCloze - Highlights to Clozes
	- ハイライト `== ==` を Cloze 化するかどうか。
	- 利用する場合にはオンにした上でノートタイプを決めてあげて Note Type Settings の Custom Regexp に正規表現パターンをコピペしてあげる。
- ID Comments
	- Anki 化される各ブロックへ自動挿入される ID を HTML のコメントタグで囲むかどうか。
- Add Obsidian Tags
	- Obsidian の `#tag` を Anki のタグとして扱うかどうか。

特に Add File Link と Add Context の設定では、File Link Field と Context Field でどのフィールドにそれらを追加するか指定する。指定しないとエラーになって正しく Anki 化できない。

![zennimage](https://storage.googleapis.com/zenn-user-upload/q4jknso982m1sizt0d00avtcpoh7)

CurlyCloze や Highlights to Clozes にしたい場合には設定を有効化した上で後で述べる Regex line のカスタムシンタックス用正規表現のパターンを次のようにコピペしてあげる。

![zennimage](https://storage.googleapis.com/zenn-user-upload/vox3mqxtdd46xpgydbgrhvryrw7u)

## Actions

\-->\[\[Data file\]\]

- `Regenerate Note Type Table`
	- ここからノートタイプの再認識を行う。
	- 新しくノートタイプを作成した場合やノートタイプを削除した場合などにはこのアクションを実行する。
- `Clear Media Cache`
	- メディアのキャッシュをクリアする。
	- メディアファイル(画像など)の名前を変更せずに内容を変えた場合などに利用する。
- `Clear File Hash Cache`
	- キャッシュされた File Hash をクリアする。

## Regex lineのスタイル(カスタムシンタックスのテンプレート)

Wiki の\[\[Regex\]\]ページに行くとカスタムシンタックスのテンプレートをリストアップしている。自分用にスタイルを最適なものを選べる。なんなら **自分専用のスタイルを完全にオリジナルで作成できる** 。Obsidian や Anki の運用にあわせたものを選ぶと良い。かならずしもカスタムシンタックスを使う必要はないが、ナレッジベースからスムーズに Anki 化したい場合には利用したほうがよいかもしれない。

## テンプレートリスト

- \[\[RemNote single-line style\]\]:
	- レムノートスタイル
	- `パラグラフ::これが答え`
- \[\[Header paragraph style\]\]
	- 目次(#)が Front になり、配下の段落が Back になる
- \[\[Question answer style\]\]
	- Q: 問題
	- A: 解答
	- 上のスタイルでどれだけスペースがあっても使えるので(正規表現)、かなり使えそう
- \[\[Neuracache flashcard style\]\]
	- `#flashcard` の前後で Front と Back が定義される。
	- [Flashcards-obsidian](https://github.com/reuseman/flashcards-obsidian) の文法がこれに当たる。
- \[\[Ruled style\]\]
	- 水平線で区切った部分で Front と Back が定義される。
- \[\[Markdown table style\]\]
	- マークダウンのテーブルスタイル
	- テーブル外のものは見ない。
	- テーブルのヘッダーが Front にそれ以外が Back になる。
	- 列は 2 つ以上でも一つとしてみなされる(二列を作っても一つの列として扱われる)。
- \[\[Cloze Paragraph style\]\]
	- Obsidian との相性が良いため個人的にこれが一番使えると思う。
	- カーリーブラケット `{}` で cloze(Anki の場合だと二重のカーリーブラケットが必要。例: `{{c1::test}}` )を作成できる。
	- Anki の cloze 文法 `{{c1::これはAnki化される}}` もこの正規表現で cloze 化される。
	- 代わりにハイライトを cloze 化することができる(別の正規表現パターンが必要)。

これらの正規表現パターンを利用する場合には上の設定の項目で説明した Note Type Settings の Custom Regexp にコピペする必要がある。

これを行うことでその正規表現パターンにマッチするブロックは自動的に指定されたノートタイプとして Anki 化される。「標準文法 `START/END` ではデフォルトのノートタイプが存在しない」ということを述べたが、このカスタムシンタックスの場合には **指定した正規表現にマッチするものは前もって設定しておいたノートタイプとしてすべて強制的にAnki化される** 。この点に注意してほしい。

## ノートの削除

上記のスタイルを利用するときは、削除のプロセスで ~~デフォルトとは違う次のコードを書く必要がある~~ 。デフォルトでは、基本文法と同様に `DELETE` を書いて削除すればよい(キーワードは変更可能)。

```markdown
DELETE  
<!--ID: 129414201900-->
```

異なるパターンが衝突しないように(かさなるようになると衝突する)気を付ける必要がある。

## Highlight-cloze style

\-->\[\[Cloze Paragraph style#Highlight-cloze style Obsidian Plugin only\]\]

ハイライト `== ==` をカーリーブラケット `{}` の代わりに cloze deletion にできる。

Regex line の設定: `((?:.+\n)*(?:.*==.*)(?:\n(?:^.{1,3}$|^.{4}(?<!<!--).*))*)`

**使い方**  
Regex line に上の正規表現パターンをコピペした上で設定項目で `CurlyCloze` の項目と `CurlyCloze - Highlights to Clozes` の項目の両方を有効化する必要がある。

### ボールドやイタリックからのClozeパターンについて

ボールドやイタリックでも Cloze 化はどうかと考えてみたが、強調表現は問題作成とは別に必要であるので、やはりハイライト→Cloze のフォーマットが良い。(ボールドやイタリックにした部分も Anki 上で表現される)

イタリックとボールドは正規表現的にも問題がありそう。 **ボールド** (`** **`)に対して *イタリック* (`* *`)となる。

## 成功パターンと失敗パターン

紹介した OTA のプラグインは Anki 化する際に失敗するパターンが多く存在するので Anki 化されるかテストした上でさらに正しい文法で書く必要がある。可能な限り失敗するパターンと成功するパターンを紹介しておく。

## ノートタイプの宣言

『デフォルトノートタイプ』の項目で書いたように標準文法の `START/END` で Anki 化する場合にはノートタイプを正しく宣言しない限り Anki 化されない。

## File Link FieldとContext Field

File Link Field と Context Field を正しく認識させた上で、 **フィールド指定を正しく行わないと** Anki 化されない場合がある。ノートタイプを Anki 側で変更したり、

## Ankiカードのフィールド追加と削除

また、フィールドの追加や削除をするたびに OTA のプラグイン側で `Regenreate Note Type` を行う必要がある。バグで再認識できないパターンがあるので再認識されるまで Anki 側で新しいノートタイプを追加するなど色々いじる必要がある。

## cloze化でのパターン

cloze 化ではいくつか Anki 化されるパターンとされないパターンが存在する。相当ややこしいので自分でテストして理解しておく必要がある。まずハイライトを cloze 化する場合には `Curly Cloze` もオンにする必要がある。ちなみに Cloze 化では一番最初のフィールドが cloze 用のフィールドとして扱われる。

- `[{これはAnki化される}]`
- `{{< これはAnki化されない >}}` ← Hugo のショートコード
- `{これはAnki化される}` ← 単一のカーリーブラケット(インライン)
- `{{c1::これはAnki化される}}` ← Anki の正しい Cloze 文法(二重カーリーブラケットに id 指定)
- `{{これはAnki化されない}}` ← Cloze 文法だが文法ミス
- `{{c1:これはAnki化されない}}` ←Cloze 文法だが文法ミス
- `{{c1|これはAnki化されない}}` ← Cloze 文法だが文法ミス
- `{ これは       Anki化される}` ← 単一のカーリーブラケット(インライン)
- `{ <br> これはAnki化される}` ← 単一のカーリーブラケット(インラインだがタグが入っている)
- `==これはAnki化される==` ← ハイライト

↓ 単一のカーリーブラケット(ブロック)

```markdown
{
これはAnki化されない
インラインじゃないとAnki化されない
}
```

↓ コードブロック内部のカーリブラケットとハイライト

```markdown
\`\`\`
コードブロック内部では{カーリーブラケット}や
==ハイライト==はAnki化されない。
\`\`\`
```

↓ ハイライトオンで Cloze 化される(Zenn の node module 内の md ファイルでひっかかってしまう)

```md
これはAnki化される 
=========
```

## 現時点での課題

## 双方向同期はできない(Anki側での内容追加について)

Obsidian\_to\_Anki であって、Anki\_to\_Obsidian ではない点に注意。Anki 側でフィールドの内容を変更したり、 **メモをとってもObsidian側には反映されることはないし** 、OTA のプラグインを走らせれば **それらの変更は更新されて上書きされるのでなかったことにされる** 。

ただし Obsidian 側で、Anki 化しているブロックの内容そのものを変更しない限りは Anki 側で追加したフィールドの内容は消されない。変更がない場合にはプラグインを実行しても Anki で追加した内容が消えることはない。

自動的にソースとなるノートへの URI リンクを挿入できるので、 **変更したい内容があればそのリンクからObsidianの元ノートそのものを変更する必要がある** 。

## フォルダ除外ができない

Obsidian\_to\_Anki に現時点で無い機能として、(1)単一ファイルのスキャン、(2)特定フォルダのスキャン除外があげられる。これによって意図しない箇所で Anki 化され、マークダウンファイルの該当箇所に ID が挿入される可能性がある。

Hugo のリポジトリを保管庫に入れている場合にはカスタムシンタックスでカーリーブラケットなどを利用してしまうと例えば、 ~~ショートコードの下にIDが挿入されてしまう可能性がある~~ (ショートコードは正規表現で引っかるが Anki 化されない)。また、テンプレート用のプラグインとして Templater などを利用している場合にも ID が挿入される可能性がある(挿入された)。

ハイライトを cloze 化したい場合にも、「必ずしもすべてのハイライトを Anki 化したいという訳ではない」ということがある。また、ハイライト用の正規表現で検索すれば分かるが、 **Zennのリポジトリのnode\_modules内のmdファイルが相当に引っかかってしまう** 。

そういったこともあるので、現時点では使用方法に気をつける必要がある。利用する際には十分にテストして、バックアップをとった上で行ってほしい(git 管理していれば大丈夫だとは思うが)。

### Folder Settingsが長くなる

これもフォルダ除外の話になるが、Zenn や Hugo のリポジトリを入れていると node modules などのフォルダがあるため、Folder Settings の Folder Table がやたら長くなってしまう。

## いくつかの打開策

フォルダ除外機能が実装されるまでの間でいくつかの打開策が考えられる。

1. reuseman の Flashcards-obsidian のプラグインを使う
2. 標準文法の `START/END` のみとりあえず使う
	- カスタムシンタックスを使う場合には正規表現検索をしたりテストして使えそうなものを選んで利用する
3. 別の保管庫で一時的に使用する  
	フォルダ除外機能が出た時点でその保管庫からノートを移動させて再びプラグインを走らせる
4. 保管庫の切り分け方法を組み替える  
	Nesetd Vault を利用して Anki 化するときのみそちらの保管庫からプラグインを走らせる

## 個人的解決策

4 番目の打開策 **Nested Vault** (ネストされた保管庫)を利用した方法で解決する。この方法であれば、特定のフォルダにあるノートのみを Anki 化できる。上で挙げたようなフォルダ除外について考える必要がなくなるのでカーリブラケットやハイライトの Cloze 化での意図しない Anki 化を防ぐことが可能。

ただし、Nested Vault は **公式非推奨の方法** なので利用する場合には自己責任の元、テストした上で利用してほしい。Nested Vault に関しては次のポストで使用法とリスクを参照。

- [Nested vaults - usage and risks](https://forum.obsidian.md/t/nested-vaults-usage-and-risks/6360/1)
- [Nested Vaults (vault within a vault)](https://forum.obsidian.md/t/nested-vaults-vault-within-a-vault/7366)

方法としては、親となる保管庫の直下に Anki 用ディレクトリを作成後、「別の保管庫を開く」コマンドで「保管庫としてフォルダを開く」を選択し Anki 用ディレクトリを選択して子となる保管庫を作成。OTA のプラグインをその保管庫内部にインストールして、その保管庫からプラグインを走らせる(親の保管庫では OTA のプラグインをアンインストールしておく)。

運用としては、編集は親の方の保管庫で行う。Anki 化したいときだけ Nested Vault(Anki 用の sub directory)を開いてプラグインを走らせる。

Anki 化したノートを移動させることは可能で、Nested Vault にインストールした OTA を削除した後に親の保管庫で OTA を再インストールして再びプラグインを走らせれば Anki カードを更新してくれる。つまり、 **フォルダ除外機能が実装されるまではNested Vaultで運用して、実装されたらNested Vaultを解体して親の保管庫で通常通りに運用** することができる。単純に Nested Vault 内部の `.obsidian/` など保管庫の設定用ファイルが入ったディレクトリ等を削除すればそのままもとの子ディレクトリとして活用することも可能。

---

## 運用戦略

Obsidian のナレッジベースから Anki 化するための戦略は今のところいくつか考えられる(なにか他の戦略があればコメントで教えてください)。

(-->\[\[Obsidian と Anki と GoodNotes 併用\]\]"2021-01-16" 分岐)

## パターン(1) Obsidianによるナレッジベース作成を主軸としたAnki化

Obsidian 内ので表現と相性の良いものがある。Obsidian でナレッジベースを作成することを主軸として考え、その過程でフラッシュカードを高速作成する。

**具体的方法**  
ハイライトやカーリーブラケットで問題にしたい部分を囲み cloze 化する。ハイライトにすると Obsidian 内での表現性を損なうことなくスムーズに Anki 化できる。

## パターン(2) Ankiカードを主軸としたObsidianでのナレッジベース作成

Anki による学習を主軸とした効率よく学習できるような Anki カードそのものを作成することを主眼に Obsidian でノートを作成する。

**具体的方法**  
Obsidian\_to\_Anki プラグイン標準文法の `START/END` ブロックを作成して各種フィールドについて記述する。

## パターン(3) そもそもObsidianからAnki化しない

Obsidian でナレッジベースを作成してから Anki 化するのに向いているものや向いていないものがあると思われるのでそもそも Obsidian から Anki 化しないという戦略もありえる。

例えば、英単語などの Anki カードを高速作成したり管理するには、Anki 標準の機能や Anki のプラグインを活用したほうがよく、Obsidian のナレッジベースと分離したほうがよい(と個人的には思う)。辞書からのソースが多い英単語などをナレッジベースにいれてしまうとノイズ増加につながる可能性がある。管理の観点からもデータベースとして扱うことの方がよいものは Anki メインでの管理がよいと思われる。

逆に数学や歴史、プログラミング等は Obsidian から Anki 化するのに向いていると思われる。英語の学習でも、長文などの管理や Writing の勉強に適しているかもしれない(そこから表現を Anki に飛ばしたりするなど)。

## パターン(4) 既存のAnkiカードにObsidianのURIリンクを挿入する

Anki のヘビーユーザーは Anki 自体にすでに大量の知識資産があると思われる。それらのよさを残したまま Obsidian との統合を図りたい場合には、Obsidian の URI リンクを既存の Anki カードのフィールドに挿入するなどの方法がよいかもしれない。

例えば、医学などの Anki カードが大量にある場合には Obsidian でのまとめノートなどの URI をフィールドのどこかに挿入するなどするのはどうだろうか？

## どのパターンが良いのか?

どのパターンがよいかはそれぞれメリットデメリットのトレードオフがあるので自分の運用(対象とするドメインや必要時間など)によって決める必要がある。

ただ、ナレッジベースそのものを [ZettelkastenやLYTで作成すること自体に学習効能はあるので](https://zettelkasten.sorenbjornstad.com/#AnkiZettelkastenIsomorphism) テスト対策などでなければパターン(1)でナレッジベースを作成することに注力しその過程で Anki 化するのがよいかもしれない。

基本的には内容やプロジェクト、時間によってどちらのパターンも両方利用したりミックスするのがよいと思われる。

## Ankiテンプレートを作成してテスト

ここまできてようやく実際に Anki 化のための Anki のノートタイプテンプレートを作成する段階にはいる(長い)。

1. Anki のテストプロファイルを作成
2. Obsidian のテスト保管庫を作成
3. Anki でテスト用のノートタイプを作成
4. 標準文法 `START/END` で実験してみる
5. カスタムシンタックスで実験してみる
	1. 検索欄で正規表現を使ってどのくらい引っかかるか見てみる
6. 自分の保管庫で実験してみる(気をつけて)
7. 運用戦略を考慮して本格導入

とりあえず標準文法 `STRAT/END` 用とカスタムシンタックスの cloze 用の二つテンプレートを作成しておけばよい。それぞれのテンプレートで用意するフィールドはだいたい 4 つ程度。Cloze となる Front フィールドは Anki 側で一番最初のフィールドにする必要がある。

1. Front: 問題文 or cloze の問題文
2. Back: 解答 or cloze の場合には追加情報など
3. SourceLink: Obsidian のソースとなるファイルへの URI リンクを挿入する
4. Context: 目次のレベルを挿入する

この 4 つを最低限用意して実験してみよう。実験なのでいい加減なテンプレートでよい。

BasicのFrontsideテンプレート

```html
{{Front}}
```

BasicのBacksideテンプレート

```html
{{FrontSide}}
<br>
<hr>

{{Back}}
<br>
{{SourceLink}}
<br>
{{Context}}
```

clozeのFrontsideテンプレート

```html
{{cloze:Front}}
```

clozeのBacksideテンプレート

```html
{{cloze:Front}}
<br>  
<hr>
{{Back}}
<br>
{{SourceLink}}
<br>
{{Context}}
```

用意できたら、Basic の方で `START/END` の実験と cloze の方でカスタムシンタックスの実験を行ってみよう。

カスタムシンタックスについては、正規表現パターンがどのくらい引っかかるか検索で知ることができる。例えば次のハイライトを Anki 化するパターンを検索してみてほしい。  
`/((?:.+\\n)\*(?:.\*==.\*)(?:\\n(?:^.{1,3}$|^.{4}(?<!<!--).\*))\*)/`

検索にはかなり引っかかると思うが、マッチしたものすべてが Anki 化されるわけではないというのがやっかいなところ。実験した結果わかったのは **インラインのものしかAnki化されない** 。

カーリーブラケット `{}` とハイライト `== ==` はインラインで使われているもののみが Anki 化される。

## Obsidianテンプレートの設定

## START/END用テンプレート

Template プラグインで標準文法 `START/END` 用のテンプレートを用意しておくと便利になる。次の 5 つのフィールドを設定したノートタイプで Anki 化する際に活用できる。

**フィールド構成**

1. Front: 問題文
2. Back: 解答
3. Title: タイトル(あってもなくてもよい)
4. SourceLink ← 設定で自動化するため書かない
5. Context ← 設定で自動化するため書かない(場合によってはこれをタイトルとして扱う)

最も簡単なテンプレート

```markdown
START
// ← ここにノートタイプを正しく記述しないとAnki化されない(利用の際はこのコメントを削除する)
Front:
Back:
Title:
END
```

## 他メモ

## Ankiカード作成日に関して

作成日が変わることはないが、変更日はちゃんとプラグインで更新があったものだけ変更される。

---

## Self

## 記事変更ログ

- 2021-01-01
	- 記事公開
- 2021-01-02
	- \[\[#運用戦略\]\]の項目に追記
	- \[\[#Zettelkasten と SRS\]\]の項目を追加
- 2021-01-03
	- \[\[#注意点\]\]と\[\[#現時点での課題\]\]の項目を追加
	- 大幅に目次構造を書き換え
- 2021-01-08
	- \[\[#成功パターンと失敗パターン\]\]の項目を追加
	- \[\[#Obsidian テンプレートの設定\]\]の項目を追加
- 2021-01-12
	- \[\[#個人的解決策\]\]の項目を追加
- 2021-04-22
	- \[\[#シンタックスハイライト\]\]の項目追加

## Meta

links:\[\[096 Obsidian MOC|Obsidian\]\]|\[\[Anki MOC|Anki\]\]|\[\[Obsidian\_to\_Anki の雑感\]\]| -->\[\[OTA 追記メモ\]\]  
url: [Obsidian\_to\_Ankiの使い方: ZettelkastenとSRSを組み合わせる](https://zenn.dev/estra/articles/integration-obsidian-and-anki)

脚注

[GitHubで編集を提案](https://github.com/yo-goto/zenn-public-repo/blob/main/articles/integration-obsidian-and-anki.md)

63

4

この記事に贈られたバッジ

63

4

[^1]: [Zettelkastenメソッド - Zettlr Docs](https://docs.zettlr.com/ja/academic/zkn-method/)