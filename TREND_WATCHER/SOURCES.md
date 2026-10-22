# Sources to scan each pass

## X (Twitter) — trusted accounts

News / markets / general signal:

1. https://x.com/ANI
2. https://x.com/shrutih29
3. https://x.com/livemint
4. https://x.com/MarketsByMint
5. https://x.com/Live_Hindustan
6. https://x.com/Shrabonti
7. https://x.com/htlifeandstyle
8. https://x.com/kaul_vivek
9. https://x.com/moneycontrolcom
10. https://x.com/BDUTT
11. https://x.com/VaibhavSisinty
12. https://x.com/testingcatalog
13. https://x.com/Inc42
14. https://x.com/He1s_Sammy
15. https://x.com/bloggersarvesh

Geopolitics:

1. https://x.com/Osint613

## X trending topics — India

- Check X's own Trending for India each pass, in addition to the accounts above.

## Google News — India

- Primary query focus: markets, personal finance, policy, business.
- Notes: `news.google.com` is not reachable from the cloud routine
  environment, and neither are most publisher pages via WebFetch — the
  discovery plan below works through WebSearch instead.

## News publishers (searched via `site:` — the main working signal)

Markets / money / business:
- livemint.com · economictimes.indiatimes.com · moneycontrol.com
- business-standard.com · thehindubusinessline.com · ndtvprofit.com
- financialexpress.com · cnbctv18.com · reuters.com (India) · inc42.com

Official / primary sources (for confirming facts):
- pib.gov.in (government press releases) · rbi.org.in · sebi.gov.in
- nseindia.com / bseindia.com (corporate announcements)

General / politics / rights:
- thehindu.com · indianexpress.com · hindustantimes.com · ndtv.com
- scroll.in · theprint.in · livelaw.in (court rulings)

## Discovery query plan (run every pass, BEFORE reading prior logs)

Run at least one query per row (12+ total). Add "today" or the date and
prefer results with a visible time stamp inside the freshness window.

| Beat | Example query |
|---|---|
| Markets | `Sensex Nifty today site:livemint.com OR site:moneycontrol.com` |
| Personal finance | `new rule from today tax bank UPI India` |
| RBI / banking | `RBI announces site:rbi.org.in OR site:business-standard.com` |
| SEBI / investing | `SEBI new rule investors site:economictimes.indiatimes.com` |
| Govt policy | `cabinet approves site:pib.gov.in` |
| Prices / inflation | `price hike India today petrol food gold` |
| Jobs / layoffs | `layoffs India company today` |
| Startups / tech | `site:inc42.com funding OR shutdown OR layoffs` |
| AI / tech policy | `India AI regulation OR MeitY today` |
| Courts / rights | `Supreme Court ruling today site:livelaw.in` |
| Consumer / viral | `India viral controversy brand today` |
| Global → India | `US tariff OR oil OR Fed impact India today` |

X accounts above are tried via `site:x.com <handle>`, but X is mostly not
indexed in near-real time — if that returns nothing, say so in one line and
move on; don't spend more queries on it.

## Review cadence for this file

Revisit this list every few weeks — accounts and sources that were early
signal last quarter may just be noise-amplifiers now. Note the last review
date here: 2026-10-22 (added publisher list + discovery query plan to fix repeated topics)
