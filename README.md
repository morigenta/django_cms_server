# HUEA Rugby Site

ラグビーサイトのCMSプロジェクトです。Django + MySQL + Nginxの構成で、Docker環境で開発を行います。

## 開発環境のセットアップ

### 必要要件
- Docker
- Docker Compose
- Git

### 環境構築手順

1. リポジトリのクローン
```bash
git clone [リポジトリURL]
cd huea_rugby_site
```

2. 環境変数ファイルの設定
```bash
cp .env.dev.example .env.dev
```
※ `.env.dev`ファイルの内容を必要に応じて修正してください。

3. Dockerコンテナの起動
```bash
docker-compose up -d --build
```

4. マイグレーションの実行（初回のみ）
```bash
docker-compose exec web python manage.py migrate
```

5. 管理者ユーザーの作成（初回のみ）
```bash
docker-compose exec web python manage.py createsuperuser
```

## 開発の進め方

### 1. モデルの作成
1. `home/models.py`にモデルを定義
2. マイグレーションファイルの作成と適用
```bash
docker-compose exec web python manage.py makemigrations
docker-compose exec web python manage.py migrate
```

### 2. 管理画面の設定
1. `home/admin.py`に管理画面の設定を追加
2. http://localhost/admin/ にアクセスして動作確認

### 3. テンプレートの作成
1. `src/templates/`ディレクトリにテンプレートファイルを作成
2. 基本レイアウト（base.html）の作成
3. 各ページのテンプレート作成

### 4. ビューの実装
1. `home/views.py`にビューを実装
2. `config/urls.py`にURLパターンを追加

### 5. 静的ファイルの管理
1. `src/static/`ディレクトリに静的ファイルを配置
2. 必要に応じて`collectstatic`を実行
```bash
docker-compose exec web python manage.py collectstatic
```

## 開発環境の操作

### コンテナの起動
```bash
docker-compose up -d
```

### コンテナの停止
```bash
docker-compose down
```

### ログの確認
```bash
docker-compose logs -f
```

### データベースの操作
```bash
docker-compose exec db mysql -u root -p
```

### Djangoコマンドの実行
```bash
docker-compose exec web python manage.py [command]
```

## デバッグ

- Django Debug Toolbarが有効になっています（開発環境のみ）
- ログは`LOG_LEVEL=DEBUG`で設定されています
- 開発サーバーは http://localhost/ でアクセス可能です

## セキュリティ注意事項

- `.env.dev`ファイルはバージョン管理に含めないでください
- 本番環境の秘密鍵やパスワードは、開発環境とは別に管理してください
- デバッグモードは開発環境でのみ有効にしてください

## 将来の拡張予定

- AWS S3との連携（静的ファイル・メディアファイルの管理）
- 本番環境へのデプロイ設定
- パフォーマンス最適化
- セキュリティ強化

## トラブルシューティング

### よくある問題と解決方法

1. マイグレーションエラー
```bash
docker-compose down -v  # ボリュームを削除
docker-compose up -d --build  # 再構築
```

2. 静的ファイルが表示されない
```bash
docker-compose exec web python manage.py collectstatic --noinput
```

3. データベース接続エラー
- コンテナの起動順序を確認
- データベースの認証情報を確認
- データベースコンテナの健全性チェック
```bash
docker-compose ps
docker-compose logs db
```

## コントリビューション

1. Issueを作成して変更内容を説明
2. ブランチを作成（feature/[機能名]）
3. 変更を実装
4. テストを実行
5. プルリクエストを作成

## ライセンス

このプロジェクトはプライベートリポジトリとして管理されています。
