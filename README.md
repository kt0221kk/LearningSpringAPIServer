# Spring Boot API Server with SQL Server

Java 17とSpring Bootを使用したRESTful APIサーバーです。SQL Serverをデータベースとして使用し、Docker環境で動作します。

## 技術スタック

- Java 17
- Spring Boot 3.2.0
- SQL Server 2022
- Docker & Docker Compose
- Maven
- Spring Data JPA
- JDBC

## プロジェクト構造

```
.
├── docker-compose.yml      # Docker環境設定
├── Dockerfile             # アプリケーションのDockerイメージ定義
├── pom.xml               # Maven依存関係管理
└── src/
    └── main/
        ├── java/com/example/api/
        │   ├── Application.java         # メインクラス
        │   ├── controller/              # RESTコントローラー
        │   │   ├── UserController.java
        │   │   └── HealthController.java
        │   ├── entity/                  # JPA エンティティ
        │   │   └── User.java
        │   ├── repository/              # データアクセス層
        │   │   └── UserRepository.java
        │   └── service/                 # ビジネスロジック層
        │       └── UserService.java
        └── resources/
            └── application.properties   # アプリケーション設定
```

## セットアップと起動方法

### 1. Dockerコンテナの起動

```bash
# Docker Composeで環境を起動
docker-compose up -d
```

これにより以下が起動します：
- SQL Server (ポート: 1433)
- Spring Boot APIサーバー (ポート: 8080)

### 2. アプリケーションの確認

```bash
# ヘルスチェック
curl http://localhost:8080/health

# APIルート
curl http://localhost:8080/
```

## API エンドポイント

### ユーザー管理API

| メソッド | エンドポイント | 説明 |
|---------|--------------|------|
| GET | `/api/users` | 全ユーザー取得 |
| GET | `/api/users/{id}` | 特定ユーザー取得 |
| POST | `/api/users` | 新規ユーザー作成 |
| PUT | `/api/users/{id}` | ユーザー情報更新 |
| DELETE | `/api/users/{id}` | ユーザー削除 |

### APIの使用例

```bash
# ユーザー作成
curl -X POST http://localhost:8080/api/users \
  -H "Content-Type: application/json" \
  -d '{
    "name": "山田太郎",
    "email": "yamada@example.com",
    "phone": "090-1234-5678"
  }'

# 全ユーザー取得
curl http://localhost:8080/api/users

# 特定ユーザー取得
curl http://localhost:8080/api/users/1

# ユーザー更新
curl -X PUT http://localhost:8080/api/users/1 \
  -H "Content-Type: application/json" \
  -d '{
    "name": "山田次郎",
    "email": "yamada-jiro@example.com",
    "phone": "090-8765-4321"
  }'

# ユーザー削除
curl -X DELETE http://localhost:8080/api/users/1
```

## 開発時の操作

### ローカルでの実行（Dockerを使わない場合）

```bash
# SQL Serverを別途起動してから
mvn spring-boot:run
```

### ビルド

```bash
# JARファイルの作成
mvn clean package

# Dockerイメージのビルド
docker build -t spring-api-server .
```

### コンテナの停止

```bash
docker-compose down

# ボリュームも削除する場合
docker-compose down -v
```

## データベース接続情報

- **ホスト**: localhost (アプリケーションからは`sqlserver`)
- **ポート**: 1433
- **データベース名**: testdb
- **ユーザー名**: sa
- **パスワード**: YourStrong@Passw0rd

## 注意事項

- 本番環境では必ずパスワードを変更してください
- SQL Serverの初回起動時は、データベース`testdb`が自動作成されません。必要に応じて手動で作成するか、初回接続時に自動作成されるまで待ってください
