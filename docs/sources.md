# Startup Data Sources Documentation

_Last Updated: August 1, 2025_

This document catalogs all available public data sources for the Startup Landscape Insight Tool, including access methods, data formats, licensing, and implementation notes.

## 🎯 Primary Data Sources

### 1. Y Combinator (YC-OSS API) - **RECOMMENDED**

- **Status**: ✅ Active, Free, No Registration Required
- **Access Method**: REST API
- **Base URL**: `https://yc-oss.github.io/api/`
- **Coverage**: 5,221+ companies across 43 batches (2005-2025)
- **Update Frequency**: Daily via GitHub Actions
- **Rate Limits**: None (static JSON hosting)
- **Licensing**: Open source data, MIT license

**Key Endpoints:**

```
https://yc-oss.github.io/api/companies/all.json          # All companies
https://yc-oss.github.io/api/batches/w25/companies.json  # Specific batch
https://yc-oss.github.io/api/industries/ai/companies.json # By industry
```

**Sample Data Structure:**

```json
{
  "id": 12345,
  "name": "Airbnb",
  "slug": "airbnb",
  "website": "https://airbnb.com",
  "one_liner": "Book accommodations around the world.",
  "long_description": "Detailed company description...",
  "batch": "W08",
  "status": "Public",
  "industry": "Consumer",
  "tags": ["marketplace", "travel"],
  "locations": ["San Francisco, CA, USA"],
  "team_size": "5000+",
  "logo_url": "https://bookface-images.s3.amazonaws.com/...",
  "black_founded": false,
  "hispanic_latino_founded": false,
  "women_founded": false,
  "hiring": true
}
```

### 2. Product Hunt Historical Dataset (Components.one)

- **Status**: ✅ Active, Free Download
- **Access Method**: Direct CSV download
- **URL**: `https://components.one/datasets/product-hunt-products`
- **Coverage**: 76,822 products (Jan 2014 - Dec 2021)
- **Update Frequency**: Static historical dataset
- **Licensing**: Research and personal use

**Data Fields:**

- Product name, description, tagline
- Launch date, vote count, comments count
- Website URL, redirect URL
- Category tags, hunter/maker info
- Featured status, premium status

### 3. Government Databases (SBIR/SBA)

- **Status**: ✅ Active, Free, API Available
- **SBIR API**: `https://www.sbir.gov/api/awards.json`
- **SBA Open Data**: `https://data.sba.gov/`
- **Coverage**: US small business awards and loans
- **Rate Limits**: 10,000 records per download
- **Licensing**: Public domain

## 📊 Secondary Sources

### 4. Kaggle Datasets

- **Y Combinator Directory**: `https://www.kaggle.com/datasets/miguelcorraljr/y-combinator-directory`
  - 4,000+ companies, MIT license
- **AngelList AI Startups**: `https://www.kaggle.com/datasets/shilpibhattacharyya/dataai-startups-dataset-from-angellist`
  - 497 AI/Data startups
- **Product Hunt 2024**: Multiple datasets available

### 5. Tech News (RSS Feeds)

- **TechCrunch Startups**: `https://techcrunch.com/category/startups/feed/`
- **TechCrunch Funding**: `https://techcrunch.com/category/funding/feed/`
- **Update Frequency**: Real-time
- **Format**: RSS/XML

## ⚠️ Restricted/Deprecated Sources

### AngelList

- **Status**: ❌ Public API Discontinued
- **Alternative**: Coresignal (commercial), GetData.io (limited)
- **Note**: Legacy API endpoints no longer functional

### Product Hunt API v2

- **Status**: ⚠️ Restricted Access
- **Limitations**: Requires approval for commercial use, user data redacted
- **Alternative**: Use historical datasets instead

### Crunchbase

- **Status**: ⚠️ Heavily Limited Free Tier
- **Note**: Basic plan requires approval, Enterprise starts at $2,000/month

## 🔧 Implementation Priority

### Phase 1 (Week 1-2): Static Datasets

1. YC-OSS API - Daily JSON ingestion
2. Product Hunt CSV - One-time historical import
3. SBIR data - Weekly government updates

### Phase 2 (Week 3-4): Real-time Sources

1. TechCrunch RSS feeds
2. GitHub trending data
3. News API integration

### Phase 3 (Future): Premium Sources

1. Coresignal AngelList data
2. Harmonic.ai startup intelligence
3. Pitchbook/CB Insights (if budget allows)

## 📝 Data Quality Notes

- **YC data**: Highest quality, most complete startup profiles
- **Product Hunt**: Great for product-level insights, trends analysis
- **Government data**: Comprehensive for US market, funding information
- **RSS feeds**: Good for real-time news, requires text processing

## 🔄 Update Schedule

- **YC-OSS**: Check daily (automated via GitHub Actions)
- **Government APIs**: Weekly downloads
- **RSS feeds**: Real-time polling every 4 hours
- **Static datasets**: Quarterly refresh check

## 🛡️ Compliance & Licensing

All selected sources comply with:

- ✅ Free/open access for research use
- ✅ No authentication barriers for core functionality
- ✅ Reasonable rate limits or no limits
- ✅ Data can be stored and analyzed locally
- ✅ Commercial use permitted or falls under fair use

---

_This document will be updated as new sources are identified or existing sources change their access policies._
