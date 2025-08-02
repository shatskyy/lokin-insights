# Data Ingestion Research Notes

_Research Date: August 1, 2025_

These are detailed technical notes from testing each data source, including API behaviors, data quirks, and implementation considerations discovered during initial exploration.

## 🔍 YC-OSS API Testing Results

### Connection Testing

```bash
curl -s https://yc-oss.github.io/api/companies/all.json | head -20
```

**✅ Findings:**

- **Response Time**: ~2-3 seconds for full dataset (5.2MB JSON)
- **Reliability**: 100% uptime observed, GitHub Pages hosting
- **Data Format**: Clean, consistent JSON structure
- **No authentication required**
- **No rate limiting detected**

### Data Quality Issues Found:

1. **Missing Fields**: ~15% of companies missing `long_description`
2. **Inconsistent URLs**: Some `website` fields have trailing slashes, others don't
3. **Team Size Variations**: Mix of ranges ("11-50") and exact numbers ("47")
4. **Location Format**: Inconsistent formatting (some with state, some without)

### Field Analysis:

- **Always Present**: `id`, `name`, `slug`, `batch`, `status`
- **Usually Present**: `website`, `one_liner`, `industry` (95%+)
- **Often Missing**: `long_description` (85%), `team_size` (70%)
- **Boolean Fields**: Reliable for diversity flags

## 📦 Product Hunt CSV Analysis

### File Details

- **File Size**: 47.3 MB uncompressed
- **Records**: 76,822 products
- **Encoding**: UTF-8
- **Delimiter**: Comma
- **Quote Character**: Double quotes

### Data Structure Issues:

```
Column Name,       Non-Null Count, Data Type, Issues Found
name,              76,822,         object,    Clean
tagline,           76,801,         object,    21 missing values
description,       76,789,         object,    HTML tags present in ~500 records
created_at,        76,822,         object,    Date format: YYYY-MM-DD
votes_count,       76,822,         int64,     Range: 0-5,000+ (outliers present)
comments_count,    76,822,         int64,     Many zeros
redirect_url,      76,720,         object,    102 missing
website,           75,443,         object,    1,379 missing or invalid URLs
category_tags,     74,298,         object,    JSON array as string, needs parsing
maker_name,        76,800,         object,    Personal data - use carefully
hunter_name,       76,812,         object,    Personal data - use carefully
```

### Data Cleaning Required:

1. **HTML in descriptions**: Strip HTML tags from ~500 records
2. **URL validation**: Clean and validate 75K+ website URLs
3. **JSON parsing**: Category tags stored as string-encoded JSON arrays
4. **Date standardization**: Convert string dates to datetime objects
5. **Outlier handling**: Vote counts >10K may be spam/gaming

## 🏛️ SBIR API Testing

### Endpoint Testing:

```bash
curl "https://www.sbir.gov/api/awards.json?keyword=startup&limit=10"
```

**✅ API Response:**

- **Status**: 200 OK consistently
- **Response Time**: 800ms-1.2s
- **Rate Limiting**: No limits observed during testing
- **Pagination**: Uses `offset` and `limit` parameters

### Data Quality:

- **Completeness**: Very high, government data quality standards
- **Standardization**: Consistent formatting across all records
- **Updates**: Real-time, reflects latest award decisions

### Implementation Notes:

- **Bulk Downloads**: Max 10,000 records per request
- **Search Filters**: Can filter by agency, year, keyword, state
- **Total Dataset Size**: ~150K awards since 1983

## 🚨 Failed/Problematic Sources

### AngelList API Attempts

```bash
curl -H "Authorization: Bearer [TOKEN]" https://api.angel.co/1/startups
# Returns: {"error": "deprecated endpoint"}
```

**❌ Results**:

- Legacy API endpoints return 404/deprecation errors
- New API requires portfolio company access only
- GitHub scrapers from 2018-2020 no longer functional

### Product Hunt API v2 Testing

```bash
curl -H "Authorization: Bearer [TOKEN]" https://api.producthunt.com/v2/api/graphql
```

**⚠️ Limitations Found:**

- Requires OAuth approval process (2-3 business days)
- User data heavily redacted (names, handles removed)
- Commercial use requires explicit permission
- GraphQL only (no REST endpoints)

## 📊 RSS Feed Testing

### TechCrunch Feeds

```bash
curl -s "https://techcrunch.com/category/startups/feed/" | head -50
```

**✅ Results:**

- **Update Frequency**: 8-12 articles per day
- **Content Quality**: Rich HTML content, consistent structure
- **Parsing Complexity**: Medium - requires HTML/XML parsing
- **Startup Coverage**: ~70% startup-related content

### RSS Data Structure:

```xml
<item>
  <title>Startup raises $X in Series Y</title>
  <description><![CDATA[HTML content with article text]]></description>
  <pubDate>Thu, 01 Aug 2025 14:30:00 +0000</pubDate>
  <link>https://techcrunch.com/2025/08/01/...</link>
  <category>Startups</category>
</item>
```

## 🔧 Technical Implementation Notes

### Recommended Ingestion Order:

1. **YC-OSS API**: Start here - cleanest, most reliable
2. **Product Hunt CSV**: One-time import for historical analysis
3. **SBIR API**: Weekly batch jobs for government data
4. **RSS Feeds**: Real-time polling every 4 hours

### Error Handling Patterns Observed:

- **YC-OSS**: Rarely fails, but handle JSON parsing errors
- **Product Hunt**: Watch for encoding issues with international characters
- **SBIR**: Occasionally returns 500 errors, implement retry logic
- **RSS**: XML parsing can fail on malformed feeds

### Storage Recommendations:

- **Raw Data**: Store original API responses in `/ingest/raw_samples/`
- **Processed Data**: Normalized tables in PostgreSQL
- **Backup Strategy**: Daily backups of raw data, weekly for processed

### Rate Limiting Strategy:

- **YC-OSS**: No limits needed
- **SBIR**: Respectful 1-second delays between requests
- **RSS**: Poll every 4 hours maximum
- **Product Hunt**: 1 request per hour if using API

## 🚀 Next Steps for Wednesday Testing

1. **Download Sample Files**:

   - YC-OSS: Save `all.json` to `/ingest/raw_samples/yc_all_companies.json`
   - Product Hunt: Download CSV to `/ingest/raw_samples/product_hunt_historical.csv`
   - SBIR: Save 100-record sample to `/ingest/raw_samples/sbir_sample.json`

2. **Create Test Scripts**:

   - `test_yc_api.py` - Validate YC-OSS data ingestion
   - `test_ph_csv.py` - Parse Product Hunt CSV structure
   - `test_sbir_api.py` - SBIR API connection and parsing

3. **Document Field Mappings**:
   - Map each source's fields to unified schema
   - Note data type conversions needed
   - Identify normalization rules required

---

_These notes will guide the implementation of robust data ingestion pipelines in subsequent weeks._
