# google shopping scraper: What Actually Works After Google Restructured Shopping, Plus Real Credit Costs and Plan Picks

Looking for a google shopping scraper usually means one of two things: you want price and product data from Google Shopping for price monitoring, arbitrage, or market research, and you want it without maintaining your own proxy pool and CAPTCHA solver. Both are still doable, but the landscape changed more than most tutorials admit. Google spent the last two years dismantling the classic Shopping surface, and several endpoints that worked in 2024 now return shutdown errors or empty responses that still bill your credits.

This guide covers what actually works, what ScraperAPI's Google Shopping endpoint delivers today, and the one thing almost no review explains properly: the real per-page cost of scraping Google domains, which is much higher than the headline credit numbers suggest.

> **Quick take:** If your project needs large-scale Google Shopping search results plus lots of other sites (Amazon, Walmart, SERP), ScraperAPI is a reasonable pick — the Hobby plan at **$49/month** covers about 4,000 Google Shopping pages, and a 7-day trial with 5,000 credits costs nothing to try. If you only need Shopping data with price filters and product detail lookups, dedicated Shopping APIs handle those better today. 👉 [Check ScraperAPI plans and start the free trial](https://www.scraperapi.com/pricing/?fp_ref=coupons)

## What changed about scraping Google Shopping

The classic Google Shopping product page is gone. For years, every scraping provider offered a product endpoint keyed by `product_id` that returned the offers and sellers page. Google shut that surface down. Following an independent benchmark published by scrape.do in mid-2026, endpoints that worked in 2024 now either return explicit shutdown errors or — worse — HTTP 200 responses full of nothing while credits are still billed.

A few facts worth internalizing before you write any code:

- **One Google Shopping search returns around 40 products.** Deep pagination is effectively unavailable. SerpApi's own documentation states that the `start` parameter is ignored and every request returns the first page of results. So plan coverage through query variety and locale changes, not page depth.
- **Google's own "Shopping Content API" won't help you.** It's for merchants uploading product feeds, not for reading search results. Result extraction runs entirely through third-party scraping tools.
- **Working product lookups rely on `page_token` harvesting.** Providers that still support product-level data extract tokens from search results and call Google's immersive product endpoint — and those tokens expire, so you harvest and consume promptly.

This context matters when evaluating any tool, because marketing pages often lag behind what Google has actually disabled.

## Ways to scrape Google Shopping, and where ScraperAPI fits

There are three realistic routes, and the right one depends on your scale and how much infrastructure you want to own.

**Route 1: Build it yourself with reverse-engineered scripts.** Open-source projects exist that reverse-engineer Google's async pagination and hidden product detail endpoints. These cost nothing in API fees but you inherit the whole maintenance burden: proxy rotation, CAPTCHAs, and the risk that Google changes something and your scraper dies mid-project.

**Route 2: Use a general scraping API.** You send a URL, the service handles proxies, browsers, and CAPTCHAs, and returns HTML or parsed JSON. ScraperAPI sits here. Its Google Shopping Scraper is a structured data endpoint that returns product names, prices, and ranking positions as clean JSON — no HTML parsing on your side.

**Route 3: Use a dedicated SERP/Shopping API.** Services like SerpApi expose Shopping as a first-class engine with query parameters for price ranges, free shipping, and on-sale filters. Generally the most convenient for pure Shopping workloads, at a higher per-query price.

ScraperAPI's pitch is breadth: one API and one billing pool covering Google Shopping, Google Search, Amazon (search, product, and offers endpoints), Walmart, Google News, Google Jobs, and more, backed by a 40-million-IP proxy pool across 50+ countries. If your roadmap includes multiple platforms, one provider for all of them simplifies billing and code. If it's Shopping-only, keep reading — there are caveats.

## ScraperAPI plans: the full price table

ScraperAPI currently publishes seven paid tiers plus a free plan. Annual billing takes 10% off the monthly price (the "annual" column below shows the discounted effective monthly rate).

| Plan | Monthly price | Annual (per month) | Credits | Concurrent threads | Purchase |
| --- | --- | --- | --- | --- | --- |
| Free plan | $0 | — | 1,000 | 5 | [Sign up for the free plan](https://www.scraperapi.com/?fp_ref=coupons) |
| Hobby | $49 | $44.10 | 100,000 | 20 | [Get Hobby](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Startup | $149 | $134.10 | 1,000,000 | 50 | [Get Startup](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Business | $299 | $269.10 | 3,000,000 | 100 | [Get Business](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Scaling | $475 | $427.50 | 5,000,000 | 200 | [Get Scaling](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Professional | $975 | $877.50 | 10,500,000 | 300 | [Get Professional](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Advanced | $1,975 | $1,777.50 | 21,500,000 | 500 | [Get Advanced](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Enterprise | Custom | Custom | 22,000,000+ | 500+ | [Talk to sales](https://www.scraperapi.com/contact-sales/?fp_ref=coupons) |

Every paid plan includes JS rendering, premium residential and mobile IPs, JSON auto-parsing, custom headers, CAPTCHA and anti-bot detection, custom sessions, automatic retries, unlimited bandwidth, and a 99.9% uptime guarantee. There's also a separate **7-day free trial with 5,000 credits, no credit card required**, which is enough to validate the Shopping endpoint before paying anything.

## The credit math nobody explains: what a Google Shopping page really costs

Here's the part most reviews skip, and it changes which plan you should buy. ScraperAPI bills by credits, but one request almost never costs one credit. The real cost depends on two things: the target domain and the feature flags you enable.

Official domain costs: a standard page costs 1 credit, Amazon costs 5, **Google and Bing (including their subdomains) cost 25**, LinkedIn costs 30, and sites behind Cloudflare, Datadome, or PerimeterX add 10 credits per request when bypassed. Feature flags stack on top — premium proxies multiply by 10, JS rendering by 10, and the ultra-premium tiers go up to 75.

Do the math on Hobby: 100,000 credits ÷ 25 = **about 4,000 Google Shopping searches per month**, roughly $0.012 per page. That's enough for monitoring a few hundred products weekly. It's nowhere near 100,000 pages, which is what the headline number implies if you assume 1 credit = 1 request. One Reddit user documented exactly this trap in the other direction: they were quoted for 60 million credits at 1 credit per Amazon request, but the 5-credit Amazon multiplier was applied, shrinking their effective plan to 12 million requests — an 80% shortfall.

Three tools keep this under control:

1. **Domain Cost Estimator** in the dashboard shows the exact credit cost for any URL before you burn quota.
2. **`max_cost` parameter** caps what a single request can consume, so a surprise flag combination can't drain your account.
3. **The Google Shopping endpoint's multiplier isn't published.** An independent benchmark estimated roughly $12.25 per 1,000 requests, but since ScraperAPI doesn't officially document that endpoint's cost, verify it in your own dashboard before committing to a volume plan.

## Hidden limits that affect which plan you pick

Several restrictions live only in plan details, not in the marketing copy:

- **Geotargeting is locked on the cheap tiers.** Hobby and Startup are limited to US and EU locations. If your price comparison spans specific countries — say, comparing German and French retailer prices — you need Business ($299) or above, which unlocks country-level targeting.
- **Analytics history is 30 days on Hobby and Startup.** Business and up get unlimited analytics. For ongoing monitoring projects, losing your usage history every month is more annoying than it sounds.
- **Credits pause, not overflow, by default.** When you run out, scraping stops until renewal. Auto-upgrade (opt-in, Hobby through Professional) moves you to the next tier automatically; pay-as-you-go continuation is only available on Scaling ($475) and above. Lower tiers that exhaust credits simply stop.
- **Concurrency is a hard 429.** Exceeding your thread limit rejects requests rather than queuing them — there's no extra charge, but your pipeline needs retry logic.

## How ScraperAPI compares for Shopping specifically

Honesty time: for a pure google shopping scraper use case, an independent 2026 benchmark of 16 providers tested the four with Shopping endpoints, and ScraperAPI's results were mixed. Search queries succeeded 100% of the time, but product-detail lookups had no working path (the classic product surface is gone and no alternative was found), price filters returned 0% support, and average response time was 9.3 seconds — the slowest of the working providers.

Here's the same benchmark's core data for context (information only; all of these have their own sites):

| Provider | Shopping search | Product lookup | Price filters | Avg response | Cost per 1K | Starting price | Free tier |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Scrape.do | 100% | 100% | 100% (via tokens) | 3.5s | $1.16 | $29/mo | 1,000 credits/mo |
| SerpApi | 100% | 100% | 100% | 0.7s | $25.00 | $25/mo | 250 searches/mo |
| ScrapingDog | 100% | 100% (via immersive endpoint) | 0% | 7.5s | $2.00 | $40/mo | 200-credit trial |
| ScraperAPI | 100% | No working path | 0% | 9.3s | ~$12.25 (estimated) | $49/mo | 1,000 credits |

SerpApi is the standout for Shopping-specific features: it supports `min_price`/`max_price`, `sort_by`, `free_shipping`, `on_sale`, and `small_business` filters, plus a device parameter, and identical cached queries are free and don't count against your monthly quota. If price filtering is central to your project, that's a genuine capability gap on ScraperAPI's side.

One warning worth copying from the benchmark: ScrapingDog still documents its legacy `google_product` endpoint, which now silently returns empty JSON with a 200 status — and bills 5 credits for it. Anyone following old tutorials into product-ID lookups burns credits on nothing. Whatever provider you pick, verify the endpoint returns real fields before building on it.

Where ScraperAPI claws back ground is the multi-site scenario. The same account, same credits, and same code pattern cover Amazon product and offers data, Walmart, general SERP, and Google News. Per-page costs for Amazon are 5 credits versus Google's 25, so Amazon-heavy workloads stretch a plan five times further.

## Who ScraperAPI makes sense for — and who should look elsewhere

Good fit:

- **Multi-platform data teams.** If you're scraping Google Shopping search results plus Amazon pricing plus SERP data, one provider and one credit pool beats stitching three services together.
- **Developers who want infrastructure off their plate.** Proxy rotation, CAPTCHA solving, and retries are handled server-side; the structured data endpoints return ready-to-use JSON.
- **Volume users on annual billing.** The 10% discount plus higher tiers' better per-credit rates add up at scale.

Poor fit:

- **Shopping-only projects that need price filters or product-level detail.** Today, ScraperAPI's Shopping search works but offers no working price-filter mechanism and no product lookup path. A dedicated Shopping API serves this better.
- **Non-technical users.** The API returns raw data; there's no dashboard workflow for building a price tracker without code. (DataPipeline exists for no-code collection, but it's positioned for enterprise-scale jobs.)
- **Anyone needing sub-$50 entry.** Hobby is the floor at $49/month. The free trial and 1,000-credit free plan are for evaluation, not production.

## Getting started without wasting credits

The trial is generous enough to answer every open question, so use it that way:

1. **Sign up for the 7-day trial** (5,000 credits, no card). 👉 [Start the free trial with 5,000 credits](https://www.scraperapi.com/?fp_ref=coupons)
2. **Check the Domain Cost Estimator** for your actual target URLs — confirm what a Google Shopping request really bills on your account.
3. **Run your real query set once.** Confirm the response contains the fields you need (names, prices, ranking positions) and note the timing.
4. **Set `max_cost`** on your requests before scaling up, so a misconfigured flag can't silently multiply costs.
5. **Test geotargeting** if you need countries outside the US and EU — you'll know immediately whether you need Business or above.

## FAQ

**Does Google Shopping scraping still work after Google's changes?**
Search-level scraping works across the major providers. What died is the standalone product page keyed by `product_id`; product detail now requires harvesting short-lived `page_token` values from search results and calling an immersive endpoint, which not every provider supports.

**How many products does one Google Shopping search return?**
About 40 per request. Pagination via the `start` parameter is ignored by Google's current layout, so deeper coverage means varying your queries and locales.

**How much does ScraperAPI cost for Google Shopping pages?**
Google domains cost 25 credits per request on paper. On Hobby ($49/month, 100,000 credits), that's about 4,000 Shopping searches monthly. Annual billing drops it to $44.10/month effectively.

**Is there a free option to test first?**
Yes — a 7-day trial with 5,000 credits and no credit card, plus a permanent free plan with 1,000 credits and 5 concurrent connections.

**What happens when I run out of credits?**
Scraping pauses until renewal by default. Auto-upgrade (opt-in on Hobby through Professional) bumps you to the next tier automatically, and pay-as-you-go continuation is available from Scaling upward.

## Final pick, by situation

If you're monitoring a few hundred products across US/EU and also touch Amazon or Walmart data, Hobby at $49 (or $44.10 on annual) is the plan, and the trial will confirm the Shopping endpoint fits your query set. If you need country-level geotargeting or unlimited analytics history, Business at $299 is where those unlock. If your entire project is Shopping price comparison with filters and product detail pages, ScraperAPI's current Shopping limitations are real, and a Shopping-specialized API — SerpApi being the strongest on filters and speed — will serve you better, even at its higher per-query cost.

One last check before paying anything: run your ten most important queries through the trial and confirm the JSON fields match what your pipeline expects. Five minutes there beats a month-long subscription to the wrong tool. When you're ready, 👉 [view all ScraperAPI plans and pricing](https://www.scraperapi.com/pricing/?fp_ref=coupons) and start with the trial credits first.
