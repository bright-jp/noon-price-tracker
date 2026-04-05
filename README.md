# Noon Price Tracker

[![Bright Data](https://img.shields.io/badge/Powered%20by-Bright%20Data-blue?style=flat-square)](https://brightdata.jp)
[![Noon Price Tracker](https://img.shields.io/badge/Noon%20Price%20Tracker-Managed%20Solution-orange?style=flat-square)](https://brightdata.jp/products/insights/price-tracker/noon)
[![Python](https://img.shields.io/badge/Python-3.9%2B-yellow?style=flat-square)](https://python.org)

[![Bright Insights Price Tracker](https://raw.githubusercontent.com/danielshashko/bright-insights-assets/main/price-tracker-hero-v2.png)](https://brightdata.jp/products/insights/price-tracker/noon)

リアルタイムのNoon価格トラッキング - 中東を代表するeコマースプラットフォーム。開始方法は2通りあります: **フルマネージド**のインテリジェンスプラットフォーム、またはBright DataのAI Scraper Builderで構築する**カスタム scraper**です。

---

## Option 1: Bright Insights - AI搭載の価格トラッキング（推奨）

**[Bright Insights](https://brightdata.jp/products/insights/price-tracker/noon)** は、Bright Dataのフルマネージドなリテールインテリジェンスプラットフォームです。scraperの構築も、インフラの保守も不要。構造化され、分析可能な価格データを、ダッシュボード、data feed、またはBIツールにそのまま配信します。

**チームがBright Insightsを選ぶ理由:**
- 🚀 **セットアップ不要** - すぐに使えるダッシュボードとdata feedで数分で利用開始
- 🤖 **AI搭載のレコメンデーション** - 会話型AIアシスタントが数百万のデータポイントを即座に実用的なインサイトへ変換
- ⚡ **リアルタイムモニタリング** - 1時間ごとから日次までの更新頻度と即時アラート（email、Slack、webhook）
- 🌍 **無制限のスケール** - あらゆるWebサイト、あらゆる地域、あらゆる更新頻度に対応
- 🔗 **プラグアンドプレイの連携** - AWS、GCP、Databricks、Snowflakeなど
- 🛡️ **フルマネージド** - Bright Dataがschema変更、サイト更新、データ品質を自動で処理

**主なユースケース:**
- ✅ 数百万のSKUにわたる**Noonの価格をリアルタイムで監視**
- ✅ **競合価格を追跡**し、値引きパターンを特定
- ✅ **価格改定を自動化**してNoon上で競争力を維持
- ✅ MAPポリシー準拠を監視し、価格違反を検出
- ✅ 競合のプロモーションと販促動向を追跡
- ✅ クリーンで正規化されたデータを動的価格設定アルゴリズムやAIモデルに直接投入

> **月額$250から - [最適なお見積もりはこちら →](https://brightdata.jp/products/insights/price-tracker/noon)**

---

## Option 2: 独自のNoon scraperを構築する

事前構築済みのNoon scraper APIがない？問題ありません。Bright Dataの**AI Scraper Builder**なら、数クリックでカスタムNoon scraperを生成できます — コーディングは不要です。

### 数分でNoon scraperを構築

**[Noon AI Scraper Builderを開く →](https://brightdata.jp/products/web-scraper/noon)**

ドメインを選択し、必要なデータ要件を記述するだけで、AI scraper builderが自動的にAPIを作成します。

1. **必要なデータを平易な英語で記述**
2. **AIが即座にscraper APIを生成**
3. **APIリクエストを実行してすぐに結果を取得**
4. **必要に応じて組み込みIDEでコードを編集**

構築が完了すると、scraperには**Web Scraper ID**（`gd_xxxxxxxxxxxx`）が付与されます — 以下のSetup手順で使用するためにコピーしてください。

### Prerequisites

- Python 3.9以上
- [Bright Data account](https://brightdata.jp)（無料トライアルあり）
- Bright Dataの**API token**（[取得方法](https://docs.brightdata.jp/general/account/account-settings#api-token)）
- Noon用の**Web Scraper ID**（上記の構築手順で取得）

### Setup

1. **このrepositoryをclone**

   ```bash
   git clone https://github.com/bright-jp/noon-price-tracker.git
   cd noon-price-tracker
   ```

2. **依存関係をインストール**

   ```bash
   pip install -r requirements.txt
   ```

3. **認証情報を設定**

   `.env.example` を `.env` にコピーし、値を入力します:

   ```bash
   cp .env.example .env
   ```

   ```env
   BRIGHTDATA_API_TOKEN=your_api_token_here
   BRIGHTDATA_DATASET_ID=your_dataset_id_here
   ```

   > **Web Scraper IDについて**
   > [AI Scraper Builder dashboard](https://brightdata.jp/products/web-scraper/noon) から取得したWeb Scraper IDを
   > `BRIGHTDATA_DATASET_ID` に貼り付けてください（形式: `gd_xxxxxxxxxxxx`）。

---

## Usage

Noon scraperの構築が完了し、Web Scraper IDを `.env` に設定すると、Pythonインターフェースは同じ方法で利用できます:

### 1. URLで特定の商品を追跡

Noonの商品URLのリストを渡して、構造化された価格データを取得します:

```python
from price_tracker import track_prices

urls = [
    "https://www.noon.com/product/sample-item-123456",
    # Add more product URLs here
]

results = track_prices(urls)
for item in results:
    print(f"{item.get('title')} - {item.get('final_price', item.get('price'))} {item.get('currency', '')}")
```

または直接実行:

```bash
python price_tracker.py
```

### 2. キーワードで商品を検索

キーワード検索に一致する商品を見つけます:

```python
from price_tracker import discover_by_keyword

results = discover_by_keyword("laptop", limit=50)
```

### 3. カテゴリURLで商品を閲覧

Noonのカテゴリページからすべての商品を収集します:

```python
from price_tracker import discover_by_category

results = discover_by_category(
    "https://noon.com/category/example",
    limit=100,
)
```

---

## Output Fields

各結果レコードには以下のフィールドが含まれます:

| Field | Description |
|-------|-------------|
| `url` | 商品ページURL |
| `title` | 商品名 / タイトル |
| `brand` | ブランドまたはメーカー |
| `initial_price` | 元の価格 / 定価 |
| `final_price` | 現在の販売価格 |
| `currency` | 通貨コード（例: USD、EUR） |
| `discount` | 値引き額または割引率 |
| `in_stock` | 商品が購入可能かどうか |
| `rating` | 平均星評価 |
| `reviews_count` | レビュー総数 |
| `seller_name` | 販売者名 |
| `images` | 商品画像URLの配列 |
| `description` | 商品説明テキスト |
| `timestamp` | データ収集タイムスタンプ |

### Sample output

```json
[
  {
    "url": "https://www.noon.com/product/sample-item-123456",
    "title": "Example Product Name",
    "brand": "Example Brand",
    "initial_price": 59.99,
    "final_price": 44.99,
    "currency": "USD",
    "discount": "25%",
    "in_stock": true,
    "rating": 4.5,
    "reviews_count": 1234,
    "images": ["https://noon.com/images/product1.jpg"],
    "description": "Product description text...",
    "timestamp": "2025-01-15T10:30:00Z"
  }
]
```

---

## Advanced Options

`trigger_collection()` 関数は、データ収集を制御するためのオプションパラメータを受け付けます:

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `limit` | integer | - | 返却するレコードの最大数 |
| `include_errors` | boolean | `true` | 結果にエラーレポートを含める |
| `notify` | string (URL) | - | スナップショットの準備完了時に呼び出すWebhook URL |
| `format` | string | `json` | 出力形式: `json`、`csv`、または`ndjson` |

オプション付きの例:

```python
from price_tracker import trigger_collection, get_results

inputs = [{"url": "https://www.noon.com/product/sample-item-123456"}]
snapshot_id = trigger_collection(inputs, limit=200, notify="https://your-webhook.com/hook")
results = get_results(snapshot_id)
```

---

## Resources

- 🌟 [Noon Price Tracker - Bright Insights (Managed)](https://brightdata.jp/products/insights/price-tracker/noon)
- 🏗️ [Build a Noon Scraper](https://brightdata.jp/products/web-scraper/noon)
- 📖 [Bright Data Web Scraper API Documentation](https://docs.brightdata.jp/scraping-automation/web-scraper-api/overview)
- 🗄️ [Web Scrapers Control Panel](https://brightdata.jp/cp/scrapers)
- 🔑 [How to get an API token](https://docs.brightdata.jp/general/account/account-settings#api-token)
- 🌐 [Bright Data Homepage](https://brightdata.jp)

---

*[Bright Data](https://brightdata.jp) を使用して構築 - 業界をリードするWebデータプラットフォーム。*