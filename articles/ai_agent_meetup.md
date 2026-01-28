---
title: "AI Agent Builders Meetup に参加しました" # 記事のタイトル
emoji: "🤖" # アイキャッチとして使われる絵文字（1文字だけ）
type: "idea" # tech: 技術記事 / idea: アイデア記事
topics: ["ai", "agent"]
published: true # 公開設定（falseにすると下書き）
# publication_name: "levtech"
---

## はじめに

1/28(水)に、「[AI Agent Builders Meetup](https://aiagentbuilders.connpass.com/event/377112/)」に参加しました。

## Google ADKのサブエージェントをAgentic workflowに移行した話 （会場スポンサーセッション）

サブエージェントにそれぞれドメインを持たせる。

Google の Agent Engine(Google ADK)

品質担保で会話がちゃんと終わるかどうかを基準。
AI側がループにならないように。

Google ADだとSub　AgentとAgent toolがある
Sub Agentは直接ユーザとやり取りできる。権限を譲渡できる。
Agent toolは親エージェント経由でサブエージェントを呼ぶ。

サブエージェントが切り替わらない不具合が発生
サブエージェントに切り替わらない問題

transfer_to_agent を呼び出す際に、コンテキストが増えると、呼び出すのを忘れてしまっていた。
死者蘇生対応（なるほど！って言わせると、サブエージェントに切り替えるようにした
だが、transfer_to_agentがテキストで出てくるようになった
プロンプトでは制御できない

Agentic Workflow(Custom Agent) に変更した
メッセージと次のAgent名をJsonで返すようにした。

マルチエージェントよりも、ワークフローの方がよかった話。

モデル2.0-flash、ほどよくバカ
Workflowの方が早くなった感じ

## Azure×サーバーレスでエレガントなエージェントワークフローが構築できることを君たちはまだ知らない

MS 技術で構築するなら2択

- Microsoft Foundry (PaaS) (旧: Azure AI Foundry)
ポータルでぽちぽちで作れる
SaaSとして、Copilot Studioもある
- Agent Framework

Foundryには、Agent Serviceなどがある
OpenAI以外のモデルもできるようになっている

Microsoft Agent Framework
オープンソース
Foundryの裏側はAgentFrameworkという話もある
Azure以外のクラウド、コンテナ、オンプレでも動く
いい感じ
OpenTelemetry, Entra IDなどにも使える
.NETとPythonで開発できる

Azureの Durable Functions

Azure Functionsとちがって、ステートフルに状態管理できる

Durable FunctionsとAgent Frameworkがいい感じに組み合わせられるようになったらしい
Durable task extensions for Microsoft Agent Framework
AIエージェントのヒストリやセッションをマネージドでできる

コード例的にも、比較的楽に作れそう
シーケンスをダッシュボードで管理できる
Human in the loop の実装もできる（承認系は、昔からあった機能

耐久性エージェントで直訳される

ワークフロー型でも、自立型も載せられる
PaaSのAgent Serviceなどもある

## Google CloudにおけるAIエージェント向けサービス選定の話

Google Cloud Vartex AI Agent Engine

Google ADKをホストできるし、Googleのエコシステムと連携できる
多くの問題は解決してくれない

Geminiであれば、APIの機能で攻撃は防いでくれる

Google Cloud Runを用いた必要な最小構成でやりましょうという提案
スモールステップ、アジャイルで開発しよう
大きく始めちゃうとステップが大きくなってしまうので、小さく始める

ElasticSearchやPostgresのベクトルembedなど

Googleでできるトッピングだと、IAPやベクトル検索、モデルアーマ、Gemini Enterpriseがある
Geminiにはgoogle検索が内包されているのでGeminiの強み

Agent FrameworksとMulti-Agent Frameworks
前者は、Google ADK, LangGraph, hookなどの細かい挙動の記述

Multi-Agentは大まかな役割とタスクの記述
CrewAI,. AutoGen, MetaGPT

今必要な機能から始めよう！
Lambda Web Adaptor

## Bedrock AgentCore 認証・認可入門

Bedrock AgentCore
Vargtex AI
Bedrock Agentcore
など、各社から出ている

AgentCore Runtime はエージェントやMCPのホスティング
AgentCore GAtewayは、APIやLambdaをMCP化
Agentcoreはunboudとoutboundで認証認可をできる、呼び出せる、アクセスしていい_
IAMやOauthが使える

AgentCore Identity

Workload Identity
Workload Access Token

ユーザの　JWTをいい感じに組み替えてくれる

Tokenの伝播
Impersonation/なりすまし
エージェントやツールがユーザと同等の剣を持ってなりすましてしまう
AgentCoreだと自然に避けれる

Token Exchangeしたい
AgentCore Gatewayの Gateway Interceptors

AgentCore Policy
AgentCoreのポリシーベース認可機能
Cedarを使用して細かい制御ができる

AgentCore Gatewayだと
多層制御できて
JWT の検証できて

## 汎用エージェントをClaude Codeで作る

ポン出しで、過小評価して使えばいいところに使っていないケースが多い

なるべく、OpenAPIやDatabaseのDDLを使う
ドメインロジックはLLMを使う

余計な指示を与えない、囲いの中で自由に動かす（AnthoropicのDocの話

## モダンUIでフルサーバーレスなAIエージェントをAmplifyとCDKでサクッとデプロイしよう

Claude Codeの、Marpと音声入力でガーっと作る

フロントエンドどうする問題

つい、Streamlitに逃げたくなってしまう、

同じ見た目になりがち
WebSocket必須で、サーバレスにならない

Amplify Gen2で使う
AWS のVercelみたいなやつ


GitHubレポで自動デプロイ
CICDが勝手に構築
Cognito認証
フロントエンドホスティング込み

npx ampx sandboxで、クラウドリソースが立ち上がる

ブランチ戦略もできる
main が本番、devが検証

CDKでカスタムリソースも内包できる
CICDでやってくれる

フロントエンドかけない
=> Claude Codeで書いてくれる

音声入力でひたすらmdを書く
30~40分、壁打ちをしまくる

コードを書く力、フルスタックになる、ClauderCodeを導く

パワポ作るマン、すごい

Strands Agents上でClaude Codeを動かす

考え事やどれだけ待たせるかをアプリ上に表示させる
（デモやアドリブの喋りがすごい

AgentCoreとAmplifyで動いている

秘書AIエージェント、すごい

音声入力

コンフルからTODOを出す

次に音声指示で、outlookを出す
フルサーバレス
Qiitaに上がっている



