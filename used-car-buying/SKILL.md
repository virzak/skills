---
name: used-car-buying
description: Research and compare used cars for a buyer - search listings, read every vehicle history report before recommending, check model-year problems and dealer fine print, and write up a comparison. Use when someone shares used-car listings, asks which used car to buy, or asks to find similar cars. Written for Canada (AutoTrader.ca, CARFAX Canada, OMVIC); adapt the sources elsewhere.
---

# Used car buying

The goal is a short list of cars that are safe to go and see, with every
claim linked to its source. Recommending a car is a claim that it is worth
the buyer's time, so nothing gets recommended before its history report has
been read.

## 1. Requirements

Get these before searching. Ask only for what the conversation has not
already answered.

- Budget, and whether they pay cash or finance.
- Must-haves: AWD or FWD, seats, cargo, and anything that has to fit (a
  rear-facing infant car seat needs rear legroom; a full-size stroller needs
  cargo space).
- Location and how far they will travel to buy.
- Body type and makes they will or will not consider.

## 2. Search

Search the big aggregator first, in the buyer's browser at their request,
at a person's pace: a few searches and listings, not a crawl. Respect the
site's robots.txt. On AutoTrader.ca the search URL takes the
make, model, region, city and maximum price in the path:

```text
https://www.autotrader.ca/cars/toyota/rav4/reg_on/cit_toronto/pr_30000?modelyearfrom=2019&modelyearto=2023
```

With browser automation, screenshots of these pages can time out; read the
DOM instead. Each result is an `<article>`. This collects a compact table:

```js
window.R = [...document.querySelectorAll('article')].map((a, n) => {
  const L = a.innerText.split('\n').map(s => s.trim()).filter(Boolean);
  const i = L.findIndex(s => /^\d{4} /.test(s));
  const d = L.findIndex(s => /km from you/.test(s));
  const link = a.querySelector('a[href*="/offers/"]');
  return {
    n, title: L[i], trim: (L[i + 1] || '').slice(0, 30),
    price: L.find(s => /^\$ ?[\d,]+$/.test(s)),
    km: L.find(s => / km$/.test(s)), dealer: L[d - 1],
    url: link ? link.href.split('?')[0] : '',
  };
});
R.map(r => [r.n, r.title, r.trim, r.price, r.km, r.dealer].join(';')).join('\n');
```

Long tool output gets truncated, so print short rows and fetch URLs for
the interesting rows by index afterwards. AutoTrader.ca pages with
`&page=2`, `&page=3` and so on.

### Cross-site comparison

The same car is usually listed on several sites, and each site shows
something the others do not. Check at least a market-analysis site and an
online retailer alongside the aggregator:

- **CarGurus.ca:** a deal rating and "$X below/above market" for each
  listing, days on market (60+ days means room to negotiate), price-drop
  history, and a drivetrain decoded from the VIN. Search URL:
  `https://www.cargurus.ca/search?zip=<postal>&distance=50&entityId=<model id>&startYear=2020&endYear=2022&maxPrice=32000&sortType=DEAL_SCORE&sortDirection=ASC`.
  Model ids (`d306` RAV4, `d2133` CX-5, `d374` Forester) appear in the
  `/Cars/l-Used-<Make>-<Model>-<City>-d<id>_L<loc>` URLs a web search
  returns. Listing pages are `https://www.cargurus.ca/details/<id>`, and
  the id is in each result card's link. CarGurus does not link history
  reports.
- **Online retailers (Clutch.ca, Canada Drives):** fixed prices, a return
  window, and a CARFAX link on each car. The report opens in a new tab
  only on a real click, not a scripted `.click()`.
- **Manufacturer certified programs:** extra warranty. Some sites gate
  their inventory behind accepting terms; do not accept terms for the
  buyer. Tell them to search it themselves or ask dealers.

Use the overlap:

- **Find a history report:** if a car on one site has no report link, find
  the same car on another site by matching km and price, and use that
  listing's report link.
- **Check the facts against each other:** trim, drivetrain and price often
  differ between sites. Trust the VIN-decoded drivetrain over a listing
  title; in testing, a car titled "AWD" on one site was FWD by its VIN.
- **Price:** use the market comparison to judge each car's price, not the
  dealer's "great price" badge.

Compare like with like: the same model across years and trims, plus its
direct rivals (for a compact SUV: RAV4, CR-V, CX-5, Rogue, Tucson,
Sportage, Corolla Cross).

## 3. History report: before anything is recommended

For every candidate on the short list:

1. Find the report link on the listing (labelled "View report" on
   AutoTrader.ca). Many dealers attach a free one.
1. Have the buyer open it, or save or paste it, and read it from there. Do
   not fetch history-report sites on your own: their robots.txt and terms
   bar automated access. Reading a report the buyer has open in their own
   browser session, at their request, is fine. If the buyer bought a report
   themselves, read their saved copy.
1. Dealer report links often open in a new window the agent cannot see.
   When the buyer asks you to read the report, take the link's `href`
   (CARFAX Canada: `https://vhr.carfax.ca/?id=...`) and open it in the
   tab you control instead of clicking. The page builds the report after
   it loads, so wait a few seconds before reading it.
1. Read the summary (VIN, last odometer, accident records yes or no,
   registration branding, service record count) and the full
   Accident/Damage section.
1. If no report is linked, say so and tell the buyer to ask the dealer for
   one. Dealer pages show CARFAX badges ("Accident Free", "One Owner").
   A CARFAX logo with no "Accident Free" badge, when the dealer's other
   cars have one, means the report has accident records: say so up front.
   In testing, such a car had four police-reported accidents. Do not treat "Verified CarFax" or "no accident" in a listing as
   evidence; one listing in testing said "no accident" over a
   police-reported collision.

A car whose report has not been read can appear in the write-up, marked
"history not checked", but it is never the recommendation, the runner-up,
or any other ranked choice. Before you name a shortlist, read the report
for every car on it; if you cannot, leave the car off and say whose report
is missing. A car that is cheap for its trim and has sat unsold for months
usually has a reason, and the report is where it shows.

Rule a car out when the report shows:

- A branded title: rebuilt, salvage, non-repairable, flood.
- Structural, undercarriage, frame or airbag damage, or a rollover or
  off-road incident.
- A single claim large relative to the car's value (roughly over 25%), or
  more than one claim.
- More than one accident, even when the claim amounts are not available.

Treat as acceptable but disclose: one minor, police-reported incident with
no or a small claim amount (a rear bump, a sideswipe).

Also note from the report:

- Registration history: provinces and states, US history, imports. Cars
  from snowbelt regions need a rust check.
- Service history: regular dealer servicing is a good sign; gaps are not.
- Odometer readings that do not line up with the listing.

## 4. Model and car checks

- Known problems for that model and year: search owner complaints and
  recalls, and name the specific issue (a transmission, a fuel pump, a
  software fix). First model years are usually the worst.
- Open recalls by VIN (Transport Canada in Canada, NHTSA in the US).
- Factory warranty left: basic and powertrain terms count from the
  original in-service date. Manufacturer-certified programs often extend
  powertrain coverage; dealer "certification" in Ontario is only a safety
  inspection.
- Drivetrain and trim: listings contradict themselves (FWD in one field,
  AWD in the title). Flag every contradiction for the buyer to confirm.

## 5. Dealer fine print

- Cash price vs finance price. A lower "finance price" only pays off if
  the loan can be paid off early without a penalty or a clawback of the
  discount. Explain the trade-off; do not give personalised financial
  advice.
- Fees on top of the price: certification, admin, registration, and
  add-ons bundled into financing.
- Ontario: a car not certified is sold "as is" under OMVIC rules and may
  not be road-worthy.
- Stock photos, "prices may change", and one key only.

## 6. Recommend

Pick one, say why in two or three lines, and give the runner-up. Then list
what the buyer must do in person:

1. Independent pre-purchase inspection at a mechanic they choose. Ask
   for a paint-thickness check of every panel: a repainted panel with no
   record on the report means damage that was never reported. In
   testing, a car with a clean CARFAX had its whole left side repainted.
   Ask the dealer why, and for the invoice; in Ontario, dealers must
   disclose damage with repair costs over $3,000.
1. Tire age, not just tread: read the four-digit DOT date code (week and
   year) on each tire. Tires older than about six years need replacing,
   and tires older than the car mean cheap or old-stock replacements.
1. Brakes: machined (resurfaced) rotors and worn pads are a cost to
   negotiate.
1. Test drive, including the conditions where the model's known problem
   shows (stop-and-go traffic for a jerky transmission).
1. Test-fit anything that must fit (car seat base, stroller).
1. Get an itemised, out-the-door price in writing.

## 7. Write it up

Record the comparison where the buyer keeps their notes: a table per round
of research with price, km, drivetrain, history verdict and a link for each
car, plus the date it was checked. Listings disappear and prices change, so
the date matters.

Keep a short "current frontrunners" list at the top, with links, holding
only cars whose report has been read. When a frontrunner is sold or ruled
out, update the list and say why. Prefer the dealer's own listing page for
links: aggregator detail pages can stop loading while the car is still for
sale.
