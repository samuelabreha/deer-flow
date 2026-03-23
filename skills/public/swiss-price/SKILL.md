---
name: swiss-price
description: Use for Swiss price comparison tasks - scraping Swiss retailers (Coop, Migros, Denner, Aldi, Lidl), product catalog management, price analysis, market audits, and comparison dashboards.
---

# Swiss Price Comparison Platform

Full-stack price comparison platform for Swiss retailers.

## Capabilities
1. Scraping - Coop, Migros, Denner, Aldi, Lidl, Globus, Manor, Qualipet, IKEA, DeinDeal, Aligro
2. Price Analysis - cross-retailer price comparison and tracking
3. Product Catalog - normalized product database with categories
4. Market Audit - retailer coverage and data quality scoring
5. Dashboard - Next.js frontend with real-time price data
6. CLI Tools - command-line scraping and comparison tools
7. Quality Gates - automated data validation pipelines

## Commands
- `npm run scrape:all` - scrape all retailers
- `npm run dev` - start development server (backend:3001 + frontend:3000)
- `npm run cli:compare` - compare prices via CLI
- `npm test` - run test suite

## Source
Application: apps/swiss-price-comparison-platform/
