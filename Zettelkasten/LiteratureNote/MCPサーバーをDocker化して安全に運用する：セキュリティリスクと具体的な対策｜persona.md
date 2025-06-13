![見出し画像](https://assets.st-note.com/production/uploads/images/189771488/rectangle_large_type_2_dfee9c7e7f0947c22c3e43cea51dc9ef.png?width=1200)

## MCPサーバーをDocker化して安全に運用する：セキュリティリスクと具体的な対策

[persona](https://note.com/persona_1)

はじめに  
本調査ノートは、2025年5月13日時点の情報に基づき、MCP（Model Context Protocol）サーバーのセキュリティリスクを軽減するためのDocker化について詳しく解説します。MCPは、AIアシスタントが外部ツールやデータソースと連携するための標準プロトコルとして、Anthropicによって開発されました（ [Model Context Protocol (MCP) an overview](https://www.philschmid.de/mcp-introduction) ）。その普及に伴い、セキュリティリスクが指摘されており、特に自動更新、依存関係の脆弱性、認証情報の盗難などが問題視されています。本稿では、これらのリスクをDockerでどのように軽減できるかを、具体的な手順と2025年の最新ベストプラクティスを交えて紹介します。

---

MCPサーバーのセキュリティリスクの詳細

MCPサーバーは、AIモデルと外部サービス間の仲介役を務めるため、以下のようなセキュリティリスクが存在します。これらのリスクは、ウェブ検索を通じて確認された内容に基づきます（ [The Security Risks of Model Context Protocol (MCP)](https://www.pillar.security/blog/the-security-risks-of-model-context-protocol-mcp) ）：

1. 自動更新によるリスク  
	MCPサーバーはGitHubなどで公開されることが多く、悪意あるコードが混入する可能性があります。信頼されるサーバーであっても、アップデートで悪意あるコードが追加されるリスクがあります。特に、npxやuvx経由で実行する場合、自動更新が適用されるため、リスクが高まります。
2. 依存関係の脆弱性  
	MCPサーバーは多くの依存パッケージを使用しており、これらのパッケージに脆弱性がある場合、サーバー全体が影響を受ける可能性があります。依存関係の管理不足は攻撃面を拡大させます（ [Everything Wrong with MCP](https://blog.sshh.io/p/everything-wrong-with-mcp) ）。
3. 権限と認証の問題  
	MCPサーバーは広範な権限を持つことが多く、認証標準の欠如が脆弱性を引き起こします。例えば、OAuthトークンが盗まれると、攻撃者はメール履歴へのアクセスや不正送信が可能になります（ [The Security Risks of Model Context Protocol (MCP)](https://www.pillar.security/blog/the-security-risks-of-model-context-protocol-mcp) ）。
4. プロンプトインジェクション攻撃  
	MCPサーバーは、AIモデルが外部ツールにアクセスする際の仲介役を務めますが、悪意のある入力でAIモデルを誤った行動に誘導するプロンプトインジェクション攻撃のリスクがあります（ [Model Context Protocol has prompt injection security problems](https://simonwillison.net/2025/Apr/9/mcp-prompt-injection/) ）。
5. サプライチェーンリスク  
	未検証のオープンソースや内部実装のMCPサーバーを使用する場合、未検証のコードが敏感なデータを扱うことになり、リスクが増大します（ [Unpacking the Security Risks of Model Context Protocol (MCP) Servers](https://www.upwind.io/feed/unpacking-the-security-risks-of-model-context-protocol-mcp-servers) ）。
6. コンテキストポイズニング  
	攻撃者は、データソースを改ざんすることで、AIモデルの出力に影響を与えることができます（ [Unpacking the Security Risks of Model Context Protocol (MCP) Servers](https://www.upwind.io/feed/unpacking-the-security-risks-of-model-context-protocol-mcp-servers) ）。

これらのリスクは、MCPサーバーの安全な運用を考える上で重要な考慮点です。

---

Dockerによるリスク軽減策

Dockerを使用することで、上記のリスクを大幅に軽減できます。以下の表は、Dockerの主要なセキュリティメリットをまとめます：

**メリット**

**説明**

環境の隔離

コンテナ内で実行し、ホストシステムへの影響を制限。侵害されても被害を封じ込め可能。

バージョンの固定

特定のバージョンをイメージに固定し、予期せぬアップデートのリスクを防ぐ。

権限の制限

必要最小限の権限のみ付与し、最小権限の原則に従うことで攻撃影響範囲を狭める。

リソースの制限

CPUやメモリの使用量を制限し、DoS攻撃等のリスクを軽減。

ネットワークの制限

必要な通信のみ許可し、不正なデータ送信や外部接続を防ぐ。

これらのメリットは、ウェブ検索結果からも確認されており、特にDockerはMCPサーバーのセキュリティ強化に適しているとされています（ [Securing Model Context Protocol: Safer Agentic AI with Containers](https://www.docker.com/blog/whats-next-for-mcp-security/) ）。

---

具体的なDocker化手順

以下では、FastAPI-MCPサーバーを例にDocker化する方法を紹介します。2025年のセキュリティベストプラクティスに基づき、以下の手順を採用します：

1. Dockerfileの作成  
	軽量なベースイメージ（例：python:3.10-slim）を使用し、非rootユーザーを作成します：
	1. dockerfile
2. requirements.txtの作成  
	バージョンを明示的に指定し、予期せぬアップデートのリスクを防ぎます：
	1. plaintext
3. docker-compose.ymlの作成  
	セキュリティ強化のための設定を追加します：
	1. yaml
4. 起動と設定  
	docker-compose buildとdocker-compose up -dでコンテナを起動し、MCPクライアントの設定ファイルを編集します。

---

2025年最新の追加セキュリティ対策

Docker化に加え、以下の対策を導入することでさらなるセキュリティ強化が可能です：

- イメージの脆弱性スキャン: CI/CDパイプラインにTrivyやSnykを統合し、定期的にスキャン（例：docker run --rm -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image fastapi-mcp）。
- シークレット管理: Docker Composeで秘密情報を管理。
- 信頼できるレジストリ: プライベートレジストリを使用し、イメージの安全性を高める。
- ネットワーク分離: 内部ネットワークを定義し、インターネットアクセスを禁止。
- ランタイム保護: Falcoのようなツールでコンテナの動作を監視。

これらの対策は、2025年のセキュリティベストプラクティスに沿ったものであり、MCPサーバーの安全性をさらに向上させます（ [Docker セキュリティベストプラクティス 2025](https://docs.docker.com/security/) ）。

---

信頼できるMCPサーバーの選択

全てのMCPサーバーが安全とは限りません。以下のサーバーが信頼性が高いとされています：

- MCP公式リポジトリ（modelcontextprotocol/servers）内のサーバー
- 接続先サービスの公式組織が実装したサーバー

それ以外のサーバーを使用する場合は、ソースコードを確認し、Docker化でリスクを軽減することが推奨されます（ [Specification - Model Context Protocol](https://modelcontextprotocol.io/specification/2025-03-26) ）。

---

既存のMCPサーバーのDocker化

すでに使用しているMCPサーバーをDocker化する場合、以下の方法が有効です：

1. ラッパーコンテナの作成: 既存のMCPサーバーをインストールして実行するためのDockerfileを作成。
2. 公式Dockerイメージの利用: Docker Hubで提供されている公式イメージを使用。
3. ソースコードのコンテナ化: ソースコードをコンテナ内に取り込んで実行。

これらの方法は、既存の環境を安全に移行するための有効な手段です（ [GitHub - docker/mcp-servers](https://github.com/docker/mcp-servers) ）。

---

結論

MCPサーバーのセキュリティリスクを軽減するためには、Dockerを活用した環境の隔離、バージョン管理、権限制限が不可欠です。本調査ノートで紹介した手順とベストプラクティスを実践することで、MCPサーバーの安全性を大幅に向上できます。ただし、完全な排除は難しいため、常に最新のセキュリティ情報を確認し、対策を更新することが重要です。

主要引用文献

- [Model Context Protocol (MCP) an overview](https://www.philschmid.de/mcp-introduction)
- [The Security Risks of Model Context Protocol (MCP)](https://www.pillar.security/blog/the-security-risks-of-model-context-protocol-mcp)
- [Everything Wrong with MCP](https://blog.sshh.io/p/everything-wrong-with-mcp)
- [Model Context Protocol has prompt injection security problems](https://simonwillison.net/2025/Apr/9/mcp-prompt-injection/)
- [Unpacking the Security Risks of Model Context Protocol (MCP) Servers](https://www.upwind.io/feed/unpacking-the-security-risks-of-model-context-protocol-mcp-servers)
- [Securing Model Context Protocol: Safer Agentic AI with Containers](https://www.docker.com/blog/whats-next-for-mcp-security/)
- [Docker セキュリティベストプラクティス 2025](https://docs.docker.com/security/)
- [Specification - Model Context Protocol](https://modelcontextprotocol.io/specification/2025-03-26)
- [GitHub - docker/mcp-servers](https://github.com/docker/mcp-servers)

![](https://assets.st-note.com/production/uploads/images/81752470/magazine_cover_landscape_sp_2b585ff247d3feeb0b8a50db804fa170.jpeg?width=200)

### ご使用感謝「みんなのフォトギャラリー」画像

- 12,022本

## コメント

MCPサーバーをDocker化して安全に運用する：セキュリティリスクと具体的な対策｜persona