---
name: swiss-price
description: Use for ANY Swiss retail or grocery price task including scraping retailers (Coop, Migros, Denner, Aldi, Lidl), comparing prices, finding best deals, analyzing promotions, generating marketing content, basket optimization, and price history analysis. Also use when user mentions Swiss products, Swiss grocery, or price comparison.
---

# Swiss Price Comparison Platform

Full-stack price comparison platform for Swiss retailers with 20+ built-in agents.

**Project Location:** Read the project at `apps/swiss-price-comparison-platform/` relative to the DeerFlow root.

## When to Use This Skill

- User asks about Swiss product prices, grocery shopping, or retailer comparison
- User wants to scrape or collect retail data
- User asks for best deals, promotions, or price drops
- User wants marketing content for retail products
- User asks about basket optimization or shopping lists
- User wants price history, trends, or anomaly detection

## Architecture Overview

Read `apps/swiss-price-comparison-platform/CLAUDE.md` for full architecture details.

**Tech Stack:** Node.js (Express backend on port 3001) + Next.js (frontend on port 3000)
**Retailers:** Migros, Coop, Denner, Aldi, Lidl, Globus, Manor, Qualipet, IKEA, DeinDeal, Aligro

## Available Workflows

### Workflow 1: Scrape Products

To scrape products from Swiss retailers:

```bash
cd apps/swiss-price-comparison-platform

# Scrape specific retailer (free, no API key needed)
node cli/demo.js --query "lait" --retailers denner,aldi,lidl

# Scrape retailers that need ZenRows API key
# (Migros, Coop require ZENROWS_API_KEY in .env)
node cli/demo.js --query "chocolat" --retailers migros,coop

# Scrape ALL retailers
npm run scrape:all
```

Output: JSON files in `data/` with product name, price, retailer, image, and URL.

### Workflow 2: Compare Prices

To compare prices across retailers:

```bash
cd apps/swiss-price-comparison-platform

# Compare a specific product
node cli/demo.js --query "nutella" --compare

# The comparison engine groups products by canonical ID
# and returns: best_price, best_retailer, all offers sorted by price
```

Read `services/comparisonEngine.js` for the comparison logic.

### Workflow 3: Start the Full Platform

```bash
cd apps/swiss-price-comparison-platform
npm install    # first time only
npm run dev    # starts backend:3001 + frontend:3000
```

### Workflow 4: Use the Built-in REST API

Once the server is running (port 3001):

```bash
# Health check
curl http://localhost:3001/health

# Search products
curl "http://localhost:3001/api/compare?q=lait&retailers=migros,coop,denner"

# Get promotions
curl "http://localhost:3001/api/promos?retailer=denner"

# Basket optimization (find cheapest retailer for a list)
curl -X POST http://localhost:3001/api/agents/basket-optimizer \
  -H "Content-Type: application/json" \
  -d '{"items": ["lait", "pain", "beurre", "oeufs"]}'

# Price anomaly detection
curl "http://localhost:3001/api/agents/anomalies?retailer=migros&hours=24"

# Generate marketing content
curl -X POST http://localhost:3001/api/agents/content/weekly-deals

# Seasonal insights
curl "http://localhost:3001/api/agents/seasonal/insights"

# Competitor landscape analysis
curl "http://localhost:3001/api/agents/competitor/landscape"
```

### Workflow 5: Analyze Data

After scraping, analyze the data:

1. Read scraped JSON files from `data/` directory
2. Use the comparison engine to find best prices
3. Detect price anomalies via the anomaly agent
4. Generate insights using the seasonal agent

### Workflow 6: Generate Marketing Content

The platform includes a content agent that can generate:

```bash
# Weekly deals article
curl -X POST http://localhost:3001/api/agents/content/weekly-deals

# Retailer comparison article
curl -X POST http://localhost:3001/api/agents/content/retailer-comparison \
  -H "Content-Type: application/json" \
  -d '{"retailers": ["migros", "coop"]}'

# Price drop article
curl -X POST http://localhost:3001/api/agents/content/price-drop
```

## Built-in Agents (20+)

| Agent | Purpose | API Endpoint |
|-------|---------|-------------|
| scrapeAgent | Run scraping jobs | POST /api/agents/scrape |
| priceAnomalyAgent | Detect unusual prices | GET /api/agents/anomalies |
| basketOptimizerAgent | Find cheapest shopping basket | POST /api/agents/basket-optimizer |
| contentAgent | Generate marketing content | POST /api/agents/content/* |
| seasonalAgent | Seasonal price trends | GET /api/agents/seasonal/* |
| competitorAgent | Competitive analysis | GET /api/agents/competitor/* |
| recommendationAgent | Product recommendations | GET /api/agents/recommendations |
| promoDetectorAgent | Find active promotions | GET /api/agents/promos |
| priceHistoryAgent | Price tracking over time | GET /api/agents/price-history |
| uptimeAgent | Retailer availability | GET /api/agents/uptime |
| costAgent | API cost tracking | GET /api/agents/costs |
| cacheAgent | Cache management | GET /api/agents/cache |
| rateLimitAgent | Rate limit status | GET /api/agents/rate-limits |
| notificationAgent | Price alerts and watchlist | POST /api/agents/watchlist |
| feedbackAgent | User feedback collection | POST /api/agents/feedback |
| a11yAgent | Accessibility checks | GET /api/agents/a11y |
| performanceAgent | Performance monitoring | GET /api/agents/performance |
| logAnalyzer | Log analysis | GET /api/agents/logs |
| reportAgent | Generate reports | POST /api/agents/report |

## End-to-End Example: Find Best Deals

Here is a complete workflow to find the best deals and create a report:

**Step 1 - Scrape:**
```bash
cd apps/swiss-price-comparison-platform
node cli/demo.js --query "lait chocolat beurre pain" --retailers denner,aldi,lidl
```

**Step 2 - Compare:**
Read the output JSON and identify products with the largest price differences across retailers.

**Step 3 - Analyze:**
```bash
curl "http://localhost:3001/api/agents/anomalies?hours=48"
curl "http://localhost:3001/api/agents/seasonal/insights"
```

**Step 4 - Generate Content:**
```bash
curl -X POST http://localhost:3001/api/agents/content/weekly-deals
```

**Step 5 - Report:**
Use the gathered data to write a summary with:
- Top 5 cheapest products per retailer
- Price comparison table
- Best overall value retailer
- Active promotions

## Key Files Reference

| File | Purpose |
|------|---------|
| `config/retailers.js` | All retailer configurations |
| `services/comparisonEngine.js` | Price comparison logic |
| `services/agents/` | All 20+ agent implementations |
| `scrapers/*.js` | Individual retailer scrapers |
| `cli/demo.js` | CLI tool for scraping and comparison |
| `server/routes/agents.js` | All agent API endpoints |
| `data/` | Scraped data output directory |
| `CLAUDE.md` | Full architecture documentation |
