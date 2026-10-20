# Shopify Products, Prices & B2B Store Leads Scraper (3-in-1)

Extract full **Shopify product catalogs, variant pricing, discount percentages, SKUs, and live stock status** — or enrich raw Shopify domains into **pitch-ready B2B agency leads** (emails, socials, min/max/avg product price, Klaviyo status, installed Ad Pixels, `missingAdPixels`, `leadScore`, and a ready-to-send `agencyPitchHook`) with **zero browser overhead**.

---

## Why Choose This Actor Over Other Shopify Scrapers?

| Feature | Our Actor (`mamang_warkop`) | Standard Shopify Scrapers |
| :--- | :--- | :--- |
| **3-in-1 Modes Included** | **`products` + `store_leads` + `collections`** in one Actor. | Separate Actors for products vs store leads. |
| **Automatic Discount % Calculation** | Computes **`discountPercent`** automatically from `price` vs `compareAtPrice`. | Raw strings only. |
| **B2B Agency Lead & Pixel Intelligence** | Extracts **emails, social links, Klaviyo installation status, Meta/TikTok/Google Ad Pixels, `missingAdPixels`, `leadScore` (0–100), and `agencyPitchHook`**. | Product data only — no B2B contact or pixel enrichment. |
| **Store Price Benchmarking** | Automatically calculates **`minPrice`**, **`maxPrice`**, and **`avgPrice`** across the store's catalog. | Not included. |
| **Zero-Browser Speed & Cost** | Direct JSON API + HTTP engine (`$1.00 / 1,000 items`) — finishes in seconds using minimal memory. | Heavy Playwright crawlers that burn compute credits. |

---

## Step-by-Step Usage Guide (Panduan Penggunaan)

### Option A: Run in Apify Console UI (Zero Coding)
1. **Enter Target Stores (`stores`):** Paste one or more Shopify store domains or URLs (`allbirds.com`, `kyliecosmetics.com`, `colourpop.com`).
2. **Select Scraping Mode (`mode`):**
   - **`Products & Pricing Catalog (products)`**: Extract individual products, prices, compare-at prices, discount %, stock availability, variant counts, SKUs, and images.
   - **`B2B Store Lead & Pixel Intelligence (store_leads)`**: Enrich each store domain into a complete B2B agency prospect record with emails, socials, price range, Klaviyo status, missing ad pixels, and a cold-email pitch hook.
   - **`Store Collections (collections)`**: Extract all public collections and item counts.
3. **Set Max Results per Store (`maxResults`):** Choose how many products/collections to fetch per store (1–5,000).
4. Click **Start**, then export your results in **CSV, Excel, or JSON**.

---

### Option B: Automate with Clay, n8n, or Make.com
```bash
curl -X POST "https://api.apify.com/v2/acts/mamang_warkop~shopify-products-and-store-leads-scraper/run-sync-get-dataset-items?token=YOUR_APIFY_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "stores": ["allbirds.com", "kyliecosmetics.com"],
    "mode": "store_leads",
    "maxResults": 10
  }'
```

---

### Option C: Python & JavaScript SDK

#### Python
```python
from apify_client import ApifyClient

client = ApifyClient("YOUR_APIFY_API_TOKEN")
run = client.actor("mamang_warkop/shopify-products-and-store-leads-scraper").call(
    run_input={
        "stores": ["allbirds.com", "kyliecosmetics.com"],
        "mode": "products",
        "maxResults": 25,
    }
)

for item in client.dataset(run["defaultDatasetId"]).iterate_items():
    print(item["storeDomain"], item["title"], item["price"], item["discountPercent"], item["inStock"])
```

---

## Input Parameters Reference

| Field | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `stores` | `array` | `["allbirds.com", "kyliecosmetics.com"]` | List of Shopify store domains or URLs. |
| `mode` | `string` | `"products"` | `"products"` (catalog & pricing), `"store_leads"` (B2B agency lead enrichment), or `"collections"`. |
| `maxResults` | `integer` | `10` | Maximum number of products or collections to extract per store (1–5,000). |

---

## Output Examples

### 1. Mode `products` (Product & Pricing Catalog)
```json
{
  "storeDomain": "allbirds.com",
  "mode": "products",
  "productId": 7340901859408,
  "title": "Women's Allbirds Flip Flop - Dusty Pink",
  "handle": "womens-allbirds-flip-flop-dusty-pink",
  "vendor": "Allbirds",
  "productType": "Shoes",
  "price": 25.0,
  "compareAtPrice": 50.0,
  "discountPercent": 50.0,
  "inStock": true,
  "variantsCount": 7,
  "skus": ["A12513W050", "A12513W060"],
  "tags": ["mens", "womens", "footwear"],
  "imageUrl": "https://cdn.shopify.com/s/files/1/1104/4168/files/...",
  "productUrl": "https://allbirds.com/products/womens-allbirds-flip-flop-dusty-pink"
}
```

### 2. Mode `store_leads` (B2B E-Commerce Agency Lead Enrichment)
```json
{
  "storeDomain": "kyliecosmetics.com",
  "mode": "store_leads",
  "title": "Kylie Cosmetics by Kylie Jenner",
  "isShopify": true,
  "leadScore": 100,
  "agencyPitchHook": "Saw kyliecosmetics.com runs Klaviyo/Shopify with avg price $26.4, but is missing Google Ads Tag conversion pixels.",
  "sampledProductsCount": 50,
  "minPrice": 15.0,
  "maxPrice": 68.0,
  "avgPrice": 26.4,
  "vendor": "Kylie Cosmetics, Kylie Skin",
  "productType": "Lip Kit, Blush, Skincare",
  "emails": ["customerservice@kyliecosmetics.com"],
  "socialLinks": {
    "instagram": "https://www.instagram.com/kyliecosmetics",
    "tiktok": "https://www.tiktok.com/@kyliecosmetics"
  },
  "detectedAdPixels": ["Meta Pixel", "TikTok Pixel", "Google Tag Manager", "Klaviyo"],
  "missingAdPixels": ["Google Ads Tag"],
  "klaviyoInstalled": true,
  "productUrl": "https://kyliecosmetics.com"
}
```

---

## Transparent Pricing
- **Pay-Per-Event:** `$0.001` per item (`$1.00 per 1,000 products or store leads`).
- Apify's free `$5.00` monthly credit covers up to **5,000 items every month for free**.
