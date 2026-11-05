# Canada Building Permits Scraper: Python example

A small, runnable example for [Canada Building Permits Scraper](https://apify.com/aurelix/canada-building-permits-scraper), an Apify Actor by Aurelix.

[![Canada Building Permits Scraper: 75-second demo video](https://api.apify.com/v2/key-value-stores/twkCCtTWzsi8fVbx5/records/canada-building-permits-scraper-demo-thumb-v1.png)](https://api.apify.com/v2/key-value-stores/twkCCtTWzsi8fVbx5/records/canada-building-permits-scraper-demo-v1.mp4)

**[▶ Watch the 75-second demo](https://api.apify.com/v2/key-value-stores/twkCCtTWzsi8fVbx5/records/canada-building-permits-scraper-demo-v1.mp4)**

## What it does

Canada Building Permits Scraper turns 10 official open-data datasets from Toronto, Calgary, Edmonton, Vancouver, Winnipeg, Kitchener and Montréal into one schema: Toronto development applications (rezoning, official plan, site plan, subdivision, condominium, part lot) and building permits, Calgary and Edmonton development and building permits, Winnipeg development permits, and Vancouver, Kitchener and Montréal issued building permits. Each record has the application number, type, category, address, postal code, ward, neighbourhood, coordinates, description, status, submitted, decision and issued dates, project value, builder or contractor company where published, and a link. Filter by dataset, days back, application type, status, ward and keyword. New-or-changed mode returns new applications and those whose status changed, such as Under Review to Approved.

## Why this over alternatives

Compared with the leading alternatives on the Apify Store (full table on the Store page):

- $3.00 per 1,000 records vs $3.50 and $5.00 for the other Canadian permit scrapers.
- The only one with development applications (rezonings, site plans) and a ward filter, across 7 Canadian cities against 5 and 4.

## When to use it

- Builders, trades and suppliers finding new projects from fresh applications and permits
- Real estate investors and brokers watching rezonings and site plans in their area
- Commercial real estate and retail teams spotting new restaurants, shops and changes of use
- Urban planners, journalists and residents tracking development in a ward

## Input example

This is the `toronto-development-applications` input used in `main.py`:

```json
{
  "cities": [
    "toronto"
  ],
  "daysBack": 60
}
```

Every input field is documented on the [Store page](https://apify.com/aurelix/canada-building-permits-scraper).

## How to run

1. Get an Apify API token from Apify Console (Settings, API & Integrations).
2. Install and run:

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
export APIFY_TOKEN=your_token_here
python main.py
```

The script starts a run of `aurelix/canada-building-permits-scraper`, prints a few fields from the first results and writes every result to `results.csv`. Lists and nested objects are stored in the CSV as JSON text. Each run is billed to your Apify account (see Pricing below).

## Sample output

One record as returned by a real run (long text is shortened):

```json
{
  "app_id": "calgary-building:BP2026-14894",
  "city": "Calgary",
  "province": "AB",
  "application_number": "BP2026-14894",
  "application_type": "Residential Improvement Project",
  "category": "1113 - Detached Garage Dwelling Unit - New",
  "address": "83G MARBANK WY NE",
  "addresses": [
    "83G MARBANK WY NE"
  ],
  "postal_code": null,
  "ward": null,
  "neighbourhood": "MARLBOROUGH",
  "description": "Additional Dwelling Unit",
  "status": "Issued Permit",
  "submitted_date": "2026-08-07",
  "decision_date": null,
  "issued_date": "2026-11-02",
  "project_value": 113393.31,
  "land_use": null,
  "permit_category": null,
  "contractor_company": "AIM BUILDER & DEVELOPMENT",
  "latitude": 51.06397737432121,
  "longitude": -113.97820309651591,
  "url": "https://data.calgary.ca/resource/c2es-76ed.json?permitnum=BP2026-14894",
  "source_url": "https://data.calgary.ca/resource/c2es-76ed.json?permitnum=BP2026-14894",
  "scraped_at": "2026-11-04T18:47:17Z"
}
```

## Export to CSV

`main.py` writes `results.csv` with the `csv` module. You can also download any run's results directly:

```
https://api.apify.com/v2/datasets/<datasetId>/items?format=csv&clean=true
```

Send your token in an `Authorization: Bearer <token>` header (or `?token=`) if the dataset is not readable without one. Other formats: `format=json`, `format=xlsx`.

## Google Sheets

- Simplest: use Apify's Google Sheets integration on the Actor or a saved task, so each run's results land in a sheet with no token in the sheet itself.
- Quick pull of one run: `=IMPORTDATA("https://api.apify.com/v2/datasets/<datasetId>/items?format=csv&clean=true")`. This only works without a token if your account lets anyone with the dataset ID read it. Do not paste an API token into a sheet formula: anyone who can view or copy the sheet can read it and use your account.

## n8n

- Apify node: choose "Run an Actor", enter `aurelix/canada-building-permits-scraper`, paste the input above, then read the run's dataset items.
- HTTP Request node: `POST https://api.apify.com/v2/acts/aurelix~canada-building-permits-scraper/run-sync-get-dataset-items` with header `Authorization: Bearer <token>` (stored as an n8n credential) and the input as the JSON body. The response is the list of results. This endpoint waits for the run to finish, so for large runs start the run asynchronously and fetch the dataset when it ends.

## Limits

- Seven cities only. Toronto Committee of Adjustment, Mississauga and Ottawa are not covered (no current open dataset).
- Vancouver, Kitchener and Montréal are issued building permits, Winnipeg issued development permits, not applications; Edmonton development permits publish a decision date only. Toronto building permits include applications not yet issued (issued_date empty) and one record per permit revision.
- Toronto adds applications to its dataset some weeks after submission, so very short look-backs can return nothing for Toronto; the default looks back 60 days.
- Toronto coordinates are converted from the city's MTM grid and are accurate to about 20 m; Toronto links point to the city's Application Information Centre or, where missing, the open-data record.
- Calgary, Edmonton, Vancouver, Winnipeg, Kitchener and Montréal links point to the open-data record, not a city web page. Toronto building permits and Edmonton building permits have no ward; Toronto building permits have no coordinates.
- Look-back is capped at 365 days (Vancouver at about 10,000 permits per run).
- Free-text descriptions can mention businesses or staff; emails and phone numbers are removed, other text is kept as published.
- Montréal records are in French as published (types, borough, description).

## Pricing

At the time of writing:

Pay per event: you pay only for what the run returns, plus a small start fee. Bronze, Silver, Gold and higher Apify plans pay less per result.

| Event | Free plan | Paid plans (Bronze, Silver, Gold) |
|---|---|---|
| `application-result` | $3.00 per 1,000 applications | $2.10 per 1,000 applications |
| `apify-actor-start` | $0.00005 per run | $0.00005 per run |

Example: 1,000 applications cost $3.00 on the free plan and $2.10 on a paid plan, plus the start fee. Apify's free plan includes $5 of credit a month, enough for about 1,600 applications.

Check the [Store page](https://apify.com/aurelix/canada-building-permits-scraper) for current prices.

## Links

- Actor: https://apify.com/aurelix/canada-building-permits-scraper
- Questions, bugs and feature requests: the Issues tab on the Store page.

## Licence

The example code in this repository is released under the MIT licence (see `LICENSE`). The Actor itself runs on Apify and is billed through your Apify account.
