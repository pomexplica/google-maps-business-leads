# Google Maps Scraper - Leads, Phones, Emails, No-Website Filter

**[Run it on Apify](https://apify.com/pomlabs/google-maps-business-leads?utm_source=github&utm_medium=readme&utm_campaign=google-maps-business-leads)**: no install, no login, free credit on a new Apify account.

Get **businesses from Google Maps** for any search, in any city or around any point: name, phone, website, full address, rating, number of reviews, opening hours, categories, coordinates and whether the owner claimed the listing. **Filter the businesses that have no website** (or only an Instagram, a WhatsApp link or a booking page), and optionally pull **e-mails and social profiles from their websites**. For Brazil, phones are flagged as mobile with a ready **WhatsApp link**, and the **company registry data (CNPJ)** can come along.

## Why this one

- **Fast and light:** reads the same data the Maps page loads, with no browser. 20 places in about 1 second, 100 places with website contacts in about 30 seconds.
- **No-website leads:** every place says what its website is (`own`, `instagram`, `facebook`, `whatsapp`, `linktree`, `booking_or_builder`) and `hasOwnWebsite`. Turn on `withoutWebsite` to keep only the businesses that still need a site. The filter is free.
- **Details included in the price:** opening hours, address split into street, neighborhood, city, state and postal code, owner claim, time zone, plus code. No extra charge per detail or per filter.
- **Contacts from the website:** e-mails, Instagram, Facebook, LinkedIn, TikTok, YouTube, X and WhatsApp from the home page and up to 2 contact pages. Charged only when something is found.
- **Brazil built in:** mobile phones flagged (`isMobilePhone`) with a `wa.me` link, and the CNPJ printed on the website enriched with legal name, status, opening date, main activity, size, capital and partners.
- **Places by link or ID:** give Google Maps links (place, `?cid=`, `maps.app.goo.gl`), place IDs (`ChIJ...`), cid numbers or hex ids and get each place in full, no search needed.
- **Place page details:** owner description, every feature Google lists (accessibility, payments, parking, crowd, children, pets...), price per person with the votes, delivery and takeout hours, order-online and menu links, owner posts, review topics with counts, cid and photo categories.
- **Photos and reviews:** full-size photo links with width and height, and reviews as their own rows: stars, text and English translation, date, reviewer with profile and number of reviews, Local Guide, food/service/atmosphere scores, review photos, owner reply with date.
- **More free filters:** category words, name must contain or equal the search, only open now (in the place's own time zone), only with a website, price level, temporarily closed. Filters never cost anything.
- **Past the 200-place limit:** give a point and an area size and turn on `scanArea`: the area is split into cells and each one is searched, up to 1,000 places per search.
- **Whole cities, states and countries:** give a city, county, state, postal code or country by name (real limits from OpenStreetMap), a GeoJSON polygon or a box of coordinates, and the area is searched cell by cell from the middle out; only places inside are kept (all Vermont dentists: 338 places in 63 towns in 74 s).
- **Places inside malls, airports and stations:** each store, restaurant and service inside as its own row, with category, rating, reviews, price, floor, hours and the directory group (Fulton Center, New York: 56 places).
- **Social profiles with followers:** the Instagram, Facebook page, YouTube channel, LinkedIn company page, TikTok and X each place links to, read for followers, following, posts, name, bio, verified badge and the e-mails shown there; the Facebook page also gives its e-mail, phone, address and recommendation rate. All six networks, in the same run.
- **Employee leads:** people who work at each business, from public LinkedIn results, with role, department, seniority and LinkedIn link, the likely e-mail from the company domain, and a department filter (owners, medical, sales, marketing...).
- **Verified e-mails:** every e-mail from the website and the social profiles checked for syntax, MX, disposable domain, role address, catch-all and the mailbox. Unknown results are free.
- **AI competitor analysis:** the places of a search ranked with a 0-100 score, strengths and weaknesses quoting their own Google reviews (every quote checked against the review it cites), social presence and recommendations per place, plus a market overview with the common complaints and the gaps a newcomer could take. In the language you ask.
- **Several cities in one run:** the same searches in each city you list, without repeating a place, each row saying which city it came from. Pass your own `userData` with each link and get it back; keep only the fields you need.

## Input

| Field | What it does |
|---|---|
| `queries` | What you would type in Google Maps: `dentist austin tx`, `barbearia nova iguaçu`. Up to 50 |
| `maxResults` | Places per search, 1 to 200 (default 20), or up to 1,000 with `scanArea`. Google Maps shows about 200 per search |
| `location` | Optional city or area added to every search |
| `language`, `countryCode` | Language of categories and hours (`en`, `pt-BR`, `es`...) and country (`us`, `br`...) |
| `latitude`, `longitude`, `radiusMeters` | Search around a point instead of a city name |
| `scanArea` | Scan the whole area around the point in cells, past the ~200 places of one search: up to 1,000 per search (central São Paulo: 1,000 restaurants in 51 s, against 141 without it) |
| `withoutWebsite` | Only businesses without their own website |
| `onlyMobilePhones` | Only Brazilian places with a mobile phone |
| `onlyWithPhone`, `minRating`, `minReviews`, `skipClosed` | More free filters |
| `extractContacts` | E-mails and social profiles from the website |
| `companyData` | Brazilian company data from the CNPJ on the website |
| `scrapeSocialMediaProfiles`, `socialNetworks` | Read the linked Instagram, Facebook, YouTube, LinkedIn company, TikTok and X profiles (followers, bio, verified, e-mails). Also accepts `{"instagrams": true, "tiktoks": true, ...}` |
| `maxLeadsPerPlace`, `leadsDepartments` | People who work at each place (0 to 10), with an optional department filter (`c_suite`, `medical_health`, `sales`, `marketing`, `operations`...) |
| `enableCompetitorAnalysis`, `maxCompetitorsToAnalyze` | AI competitor analysis of the places found (2 to 100, default 30): rows `competitor-analysis` per place and one `market-overview` |
| `verifyEmails` | Check every e-mail found (syntax, MX, disposable, role, catch-all, mailbox) |
| `startUrls` | Google Maps place links, `?cid=` links, short links, place IDs, cid numbers or hex ids, up to 500. Search links run as searches |
| `scrapePlaceDetails` | Read each place page for the full details (always on for links) |
| `maxImages` | Full-size photo links per place (0 to 200) |
| `maxReviews`, `reviewsStartDate` | Reviews per place as rows (today up to the 5 most relevant Google shows per place), and only those since a date |
| `categoryFilterWords` | Keep a place only if a category contains one of the words (`pizza` keeps `Pizza restaurant`) |
| `searchMatching` | `all`, `only_includes` (name has every word of the search) or `only_exact` |
| `openNow`, `onlyWithWebsite`, `priceLevels`, `skipTemporarilyClosed` | More free filters |
| `locations` | More cities or areas: every search runs in each one, without repeating a place. Up to 50 |
| `city`, `county`, `state`, `postalCode`, `searchWholeCountry` | Search inside the real limits of a place by name (OpenStreetMap), with `countryCode`; up to 1,000 places per search |
| `customGeolocation` | A GeoJSON Polygon or MultiPolygon (`[longitude, latitude]`), or a Point with `radiusKm`: only places inside |
| `boundingBox` | `{"south": 40.74, "west": -74.0, "north": 40.76, "east": -73.98}`: only places inside |
| `scrapePlacesInside`, `maxPlacesInside` | For malls, airports and stations, each place inside as its own row (up to 100 per place by default, charged as places) |
| `fields` | Keep only these output fields |

## Output

```json
{
  "type": "place",
  "query": "escritório de contabilidade nova iguaçu",
  "rank": 2,
  "name": "Andrade Contabilidade | Contabilidade em Nova Iguaçu | Escritório de Contabilidade",
  "category": "Escritório de contabilidade",
  "rating": 4.9,
  "reviewsCount": 182,
  "phone": "(21) 96482-0925",
  "phoneInternational": "+55 21 96482-0925",
  "isMobilePhone": true,
  "whatsapp": "https://wa.me/5521964820925",
  "website": "https://www.andradecont.com.br/",
  "websiteType": "own",
  "hasOwnWebsite": true,
  "street": "R. Dr. Barros Júnior, 125 - Sobrado",
  "neighborhood": "Centro",
  "city": "Nova Iguaçu",
  "state": "Rio de Janeiro",
  "postalCode": "26210-301",
  "countryCode": "BR",
  "latitude": -22.7571516,
  "longitude": -43.4516614,
  "openingHours": {"segunda-feira": ["09:00–18:00"], "sábado": ["Fechado"]},
  "claimedByOwner": true,
  "placeId": "ChIJbToWlZhnmQARVgXVJ2PezIk",
  "googleMapsUrl": "https://www.google.com/maps/place/?q=place_id:ChIJbToWlZhnmQARVgXVJ2PezIk",
  "emails": ["contato@andradecont.com.br"],
  "contactsFound": true,
  "company": {
    "cnpj": "10997909000110",
    "legalName": "ANDRADE ASSESSORIA CONTABIL E EMPRESARIAL LTDA",
    "registrationStatus": "ATIVA",
    "openedOn": "2009-07-22",
    "mainActivity": "6920601 Atividades de contabilidade",
    "companySize": "MICRO EMPRESA",
    "partners": [{"name": "...", "role": "Sócio-Administrador"}],
    "nameMatchesPlace": true
  },
  "status": "ok"
}
```

Searches that fail (Google refusing, nothing found with the filters) come as a row with `type: "error"` and the reason, and are not charged.

## Use cases

- **Lead generation:** local businesses with phone, WhatsApp and e-mail, by niche and city.
- **Web design and marketing agencies:** businesses with no website, or only an Instagram, ready to be offered a site.
- **Market research:** how many competitors, how they are rated and where they are.
- **Brazilian B2B:** CNPJ, company size and partners of local businesses.

## Pricing

- **$3.60 per 1,000 places**, with all the details and filters included (less on higher Apify plans).
- **$1.80 per 1,000 places with contacts found** on the website (only when at least one e-mail or profile is found).
- **$4.50 per 1,000 Brazilian company records** (CNPJ found and enriched).
- **$1.80 per 1,000 place pages read** (details, images or reviews on, or a place given by link).
- **$0.45 per 1,000 reviews** and **$0.45 per 1,000 photo links**.
- **Social profile read:** $7.20 per 1,000 on Bronze ($5.40 on Gold). **Employee lead:** $4.50 per 1,000 ($3.60 on Gold). **E-mail verified:** $3.60 per 1,000 ($1.80 on Gold), conclusive results only. **Competitor analyzed by AI:** $18 per 1,000 places on Bronze ($14.40 on Gold); the market overview is free.
- No charge per run.

## Notes

- Google Maps lists about 200 places per search. For more, use `scanArea` with a point, or split by neighborhood.
- A run has about 80 seconds per search; on big scans with e-mails on, the places whose website was not visited in time come with `contactsStatus: "not_checked"` and are not charged for contacts.
- Since December 2025 Google shows the full review list only to logged-in users; without login each place page gives the 5 most relevant reviews, which is what `maxReviews` delivers today.
- Websites that are down or refuse visitors come with `contactsStatus: "site_unreachable"` and are not charged for contacts.
