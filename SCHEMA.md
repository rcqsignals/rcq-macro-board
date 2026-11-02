# Data contract — `us-macro` v1

Public, no-cache JSON at `https://macro.rcqsignals.com/data/<file>.json`. Refreshed each weekday ~5:47 AM PT by a scheduled task; `meta.json.updatedAt` is the run time (stale if > 80 h old, which covers weekends). Field names are stable; new fields may be added.

## meta.json
`{ "updatedAt": ISO-8601 UTC, "by": "Scheduled refresh" | ... }`

## metrics.json — array, one row per metric (24 rows)
| field | type | meaning |
|---|---|---|
| id | string | stable key: fedfunds, effr, ust2y, ust10y, ust30y, curve10s2s, cpi, corecpi, pce, corepce, m2, gdp, unemp, payrolls, hyoas, igoas, sloos, dxy, deficit, debtpublic, grossdebt, wti, brent, natgas |
| category | string | rates · curve · inflation · growth · labor · liquidity · credit · dollar · fiscal · energy |
| value | number \| {lower,upper} \| null | latest reading in `unit`; fedfunds is a range; sloos is null (qualitative) |
| unit | string | pct · pct_yoy · pct_saar · bp · usd_bn · jobs_k · index · usd_bbl · usd_mmbtu · text |
| prior | same shape as value \| null | prior reading, described by `prior_label` |
| prior_label | string \| null | e.g. "Aug 2026", "1 month earlier" |
| changes | array of {vs, value, unit} | vs: 1m · 1w · 3d · ytd · yoy · mom · mtd · prior_fy · since_YYYY-MM-DD; unit pct_pt · bp · pct · usd_bn |
| as_of_date | YYYY-MM-DD | release or market-close date of `value` |
| period_code | string | reference period: YYYY-MM, YYYY-Qn, FYYYYY, or a date for daily series |
| next_release_date | YYYY-MM-DD \| null | next scheduled print (matches calendar.json) |
| redistribution | string | public_domain · licensed_restricted (ICE BofA OAS, DXY, exchange futures/gas settles — do not resell) |
| tone | string | good · warn · bad · neutral, from the view of an investor wanting cheaper, more available capital |
| display, change, period, asOf, source, next | string | human-readable text used by the page |

## history.json — array of {series, date: YYYY-MM-DD, value: number}
Series: ust10y (pct), hyoas (pct, ×100 = bp), dxy, wti, brent (daily, ~45 days); m2 (usd_bn), gdp (pct_saar, date = quarter start), corepce (pct_yoy) — last 5–8 prints.

## curve.json — array of {tenor: 1M|3M|6M|1Y|2Y|10Y|30Y, yield_pct: number}

## calendar.json — array, unique on (date, release)
{date: YYYY-MM-DD, timeET: "HH:MM" 24h Eastern, release, period, moves, agency, impact: high|med, status: Scheduled|Estimated}

## regime.json — array of {pillar, read, tone} for 9 pillars
## brief.json — [{headline, asOf}]
`regime` and `brief` are analyst judgment written at each refresh, not data.
