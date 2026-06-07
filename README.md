# illust_manager v3

## README

- 名前の通り画像管理システム
- ローカルで動作します。事故が怖いのでWeb上に公開とかはしない予定
- 制作時期: 2024年～ (gptがメタになってから)

## 概要

- いろいろ画像管理システム
  - 各種SNSから画像を自動DLする
  - ローカルの画像からタグを生成する
  - 画像とタグを紐づけてDBに格納する
  - etc

## Image Manager

ローカルにある画像に対して、[Deepdanbooru](https://github.com/KichangKim/DeepDanbooru)を用いて、
イラストにタグ付与を行い、DBに紐づける形で登録します。
以下はWIP
- タグを基準にした画像ソート
- 手動で画像にタグを付与してDeepdanbooruに学習させる
  - ローカルのDeepdanbooruに対して学習させるのみでpublicにはしない

## ImageDLer

- Twitter(現X)・pixivの画像をまとめて取得・ダウンロードするツールです。
- 指定したユーザーIDの、Twitterは「いいね」、pixivは「ブックマーク」・「ポスト」の画像一覧を取得できます。
- pixivの投稿取得には[pixivpy](https://github.com/upbit/pixivpy)モジュールを利用しています。

## 技術スタック

| レイヤー | 技術 |
|----------|------|
| フロントエンド | Vue.js 3 / TypeScript / Vuetify 3 / Pinia / axios |
| バックエンド | Python / Flask / SQLAlchemy / Flask-CORS |
| AI/ML | TensorFlow 2.17 / DeepDanbooru 1.0 / Keras / scikit-learn |
| スクレイピング | Selenium / pixivpy3 / cloudscraper |
| データベース | MySQL 8.0 / mysql-connector-python |
| インフラ | Docker Compose |

## セットアップ

```bash
# .env を作成して設定
cp .env.example .env

# Docker で起動
docker-compose up -d
```

```bash
# ローカル開発（Dockerなし）
npm install && npm run serve  # フロントエンド

pip install -r requirements.txt
python -m flask run --reload  # バックエンド（別ターミナル）
```

## 環境変数（.env）

| 変数 | 説明 |
|------|------|
| `SECRET_KEY` | Flask セッションキー |
| `PIXIVPY_REFRESH_TOKEN` | Pixiv OAuth リフレッシュトークン |
| `TEL` | Twitter 2FA 用電話番号 |

## ディレクトリ構成

```
old-illust_manager_v3/
├── api/
│   ├── controller/       # Flask Blueprint (twitter, pixiv, image, tag 等)
│   ├── service/          # ビジネスロジック (twitter, pixiv, download 等)
│   ├── model/            # SQLAlchemy モデル
│   ├── deepdanbooru/     # DeepDanbooru タグ付け
│   └── driver/           # Pixiv 認証・Selenium
├── src/
│   └── views/            # imagedler.vue, imageManager.vue 等
├── app.py                # Flask エントリポイント
├── requirements.txt
└── docker-compose.yaml
```
