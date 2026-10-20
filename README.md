# Shopify & WooCommerce Products Scraper — Python, Node.js & cURL examples

Get **every product of any Shopify or WooCommerce store** — price, compare-at/sale price, discount, **variants with SKU and stock**, images, tags, description — and **track changes** between runs (`new`, `price_down`, `price_up`, `back_in_stock`, `out_of_stock`). **$0.50 per 1,000 products**.

Hosted on Apify: **[Shopify & WooCommerce Products Scraper](https://apify.com/tidydata/shopify-woocommerce-scraper)**. These examples show how to call it from your code; the Actor runs in the cloud, no API key for the source site, no proxies to manage.

## Python

```bash
pip install apify-client
export APIFY_TOKEN=...   # free account at apify.com → Settings → API & Integrations
```

```python
import os
from apify_client import ApifyClient

client = ApifyClient(os.environ["APIFY_TOKEN"])
run = client.actor("tidydata/shopify-woocommerce-scraper").call(run_input={
    "stores": [
        "https://www.allbirds.com",
        "https://kyliecosmetics.com/collections/all"
    ],
    "maxProductsPerStore": 1000
})
for item in client.dataset(run["defaultDatasetId"]).iterate_items():
    print({k: item.get(k) for k in ['storeName', 'title', 'price', 'compareAtPrice', 'available']})
```

## Node.js

```javascript
import { ApifyClient } from 'apify-client';

const client = new ApifyClient({ token: process.env.APIFY_TOKEN });
const run = await client.actor('tidydata/shopify-woocommerce-scraper').call({"stores": ["https://www.allbirds.com"], "trackChanges": true, "onlyChanges": true});
const { items } = await client.dataset(run.defaultDatasetId).listItems();
console.log(items.length, items[0]);
```

## cURL (one call, results as JSON)

```bash
curl -s -X POST "https://api.apify.com/v2/acts/tidydata~shopify-woocommerce-scraper/run-sync-get-dataset-items?token=$APIFY_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"stores": ["https://www.allbirds.com"], "trackChanges": true, "onlyChanges": true}'
```

## More TidyData Actors

[YouTube Transcript](https://apify.com/tidydata/youtube-transcript-scraper) · [YouTube Comments](https://apify.com/tidydata/youtube-comments-scraper) · [YouTube Scraper](https://apify.com/tidydata/youtube-channel-scraper) · [Website Contacts](https://apify.com/tidydata/website-contact-extractor) · [Google News](https://apify.com/tidydata/google-news-scraper) · [App Reviews](https://apify.com/tidydata/app-reviews-scraper) · [Merge & Dedupe](https://apify.com/tidydata/merge-dedupe-datasets) · [all of them](https://github.com/tidydata-dev)

Examples are MIT-licensed.
