
# RPC_Practice

## 目的
gRPCを用いたRPC（Remote Procedure Call）の仕組みを、サンプル実装を通じて学ぶことを目的としています。

## 実験内容
- gRPCの基本的な仕組みを理解する
- PythonでgRPCサーバ・クライアントを実装し、計算サービス（加算など）をRPCでやりとりする
- protoファイルを用いたインターフェース定義と、その共有方法を学ぶ
- Docker/Docker Composeを使い、サーバ・クライアントをコンテナで分離して動作させる

## 実行方法
1. 必要なファイル構成でリポジトリをclone、またはダウンロードします。
2. プロジェクトルートで以下を実行します：
	 ```sh
	 docker compose up --build
	 ```
3. サーバ・クライアントがそれぞれコンテナで起動し、gRPC通信が行われます。

## 構成

```
.
├── client
│   ├── client.py
│   ├── Dockerfile
│   └── requirements.txt
├── docker-compose.yml
├── proto
│   └── calculator.proto
├── README.md
└── server
		├── Dockerfile
		├── requirements.txt
		└── server.py
```

- `proto/calculator.proto` ... サービス定義（加算など）
- `server/` ... gRPCサーバ実装
- `client/` ... gRPCクライアント実装
- `docker-compose.yml` ... サーバ・クライアントのビルド・起動設定

## gRPCプロジェクトのビルド時の注意点

`proto` ディレクトリはサーバ・クライアント両方から利用します。
Dockerビルド時は `build.context` をプロジェクトルート（`.`）にし、`dockerfile` で `client/Dockerfile` や `server/Dockerfile` を指定してください。

### docker-compose.yml例
```yaml
services:
	server:
		build:
			context: .
			dockerfile: server/Dockerfile
		container_name: grpc-server
		ports:
			- "50051:50051"

	client:
		build:
			context: .
			dockerfile: client/Dockerfile
		container_name: grpc-client
		depends_on:
			- server
```

### Dockerfile例
```dockerfile
# server/Dockerfile, client/Dockerfile 共通
COPY proto /proto
```

このようにすることで、`proto` ディレクトリを正しくイメージに含めることができます。
