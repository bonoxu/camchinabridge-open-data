# Cambridge China Bridge open data

Four small, machine-readable datasets for importers, exporters, researchers, journalists and AI tools, published by [Cambridge China Bridge](https://camchinabridge.co.uk/), a China sourcing service based in Cambridge, UK.

| File | What it is | Rows | Licence |
| --- | --- | --- | --- |
| `data/uk-china-import-pulse.csv` | UK imports from China: latest quarter vs the same quarter a year earlier, by HS chapter and heading, plus quarterly totals since 2019 | 59 | Open Government Licence v3.0 (HMRC data, Crown copyright) |
| `data/uk-china-export-pulse.csv` | UK exports to China: latest quarter vs the same quarter a year earlier, by HS chapter and heading, precious metals (HS 71) separately, plus quarterly totals since 2019 | 90 | Open Government Licence v3.0 (HMRC data, Crown copyright) |
| `data/china-eu-trade-pulse.csv` | Imports from China into each EU member state (26 countries, `country_code` column): latest quarter vs a year earlier, by HS chapter and heading, plus quarterly totals since 2019 | 1534 | Source: Eurostat (re-use with acknowledgement) |
| `data/china-holidays-factory-closures.csv` | China's public holidays, make-up working days (调休) and estimated Chinese New Year factory closures | 19 | CC BY 4.0 (compilation and estimates); official dates are public facts |

All four are described in `datapackage.json` ([Frictionless Data](https://specs.frictionlessdata.io/) Data Package), so tools such as `frictionless validate datapackage.json` can read the schema directly.

Version **2026.10.1** (Import, Export and China–EU Pulses cover Q2 2026 vs Q2 2025; holiday dates checked 27 September 2026).

## 1. UK-China Import Pulse

Live page: <https://camchinabridge.co.uk/uk-china-import-pulse/>
Live CSV (this file is byte-identical to it): <https://camchinabridge.co.uk/uk-china-import-pulse.csv>

The latest calendar quarter that HMRC has fully published, compared with the same quarter a year earlier:

- the headline total of UK imports from China;
- the 12 largest HS2 chapters;
- the 8 fastest-rising and 8 steepest-falling HS4 headings (headings under £10m in both quarters are left out as noise);
- quarterly totals from Q1 2019, the last full year before Covid.

Chapter 99 and unclassified lines are left out of every total. Values are pounds sterling at current prices, as published by HMRC; HMRC revises recent months, so figures can change between versions.

The file starts with three comment lines beginning `#` (source, citation, OGL attribution). The Data Package declares `commentChar: "#"`. In a spreadsheet, delete those three lines or skip them on import.

| Field | Type | Meaning |
| --- | --- | --- |
| `section` | text | `total`, `largest chapter (HS2)`, `fastest-rising heading (HS4)`, `steepest-falling heading (HS4)` or `quarterly total` |
| `code` | text | HS2 chapter or HS4 heading code (text, so leading zeros survive); empty for totals |
| `description` | text | HMRC commodity description, or "All UK imports from China" |
| `period` | text | Quarter the value covers, e.g. `Q2 2026` |
| `value_gbp` | integer | Imports from China in that quarter, £ |
| `previous_period` | text | Comparison quarter (same quarter a year earlier); empty for quarterly totals |
| `previous_value_gbp` | integer | Imports in the comparison quarter, £ |
| `change_pct` | number | % change from previous to current, rounded to 1 decimal place |
| `gbp_per_kg` | number | Value density: £ per kg of net mass in the current quarter; empty for totals |

**Source:** HM Revenue & Customs, Overseas Trade Statistics (OTS), queried through the uktradeinfo OData API (`api.uktradeinfo.com/OTS`): partner country China (CountryId 720), non-EU imports (FlowTypeId 3). <https://www.uktradeinfo.com/>

**Attribution required (OGL v3.0):**

> Contains HMRC data © Crown copyright 2026, licensed under the Open Government Licence v3.0.

## 2. UK-China Export Pulse

Live page: <https://camchinabridge.co.uk/uk-china-export-pulse/> (中文: <https://camchinabridge.co.uk/zh/uk-china-export-pulse/>)
Live CSV (this file is byte-identical to it): <https://camchinabridge.co.uk/uk-china-export-pulse.csv>

The same structure as the Import Pulse, for UK exports to China (HMRC non-EU exports, FlowTypeId 4). One difference: precious metals and stones (HS chapter 71, almost all non-monetary gold) are reported in their own rows (`precious metals (HS2 71, reported separately)` and `quarterly precious metals (HS2 71)`) and left out of the headline total, the chapters and the movers. In some quarters gold is worth more than every other export to China together, and it swings by billions from one quarter to the next.

Fields are the same as the Import Pulse (`value_gbp`, `previous_value_gbp`, `change_pct`, `gbp_per_kg` …), with two extra `section` values for the precious-metal rows.

**Source:** HM Revenue & Customs, Overseas Trade Statistics (OTS), uktradeinfo OData API: partner country China (CountryId 720), non-EU exports (FlowTypeId 4). <https://www.uktradeinfo.com/>

**Attribution required (OGL v3.0):**

> Contains HMRC data © Crown copyright 2026, licensed under the Open Government Licence v3.0.

## 3. China-EU Trade Pulse

Live hub: <https://camchinabridge.co.uk/china-eu-trade-pulse/> (one page per member state, e.g. <https://camchinabridge.co.uk/china-eu-trade-pulse/ireland/>; 中文: <https://camchinabridge.co.uk/zh/china-eu-trade-pulse/>)
Live CSV (this file is byte-identical to it): <https://camchinabridge.co.uk/china-eu-trade-pulse.csv>

Imports from China into each EU member state, from Eurostat's Comext database, one block of rows per country. For each country: the latest calendar quarter Eurostat has fully published against the same quarter a year earlier, the total, the 12 largest HS2 chapters, the 8 fastest-rising and 8 steepest-falling HS4 headings, and quarterly totals from Q1 2019. A heading counts as a mover only if it reached 0.25% of that country's quarter total (between €1m and €10m) in both quarters. Chapters 99 (other products) and 00 (confidential totals) are excluded.

Member states whose series is too sparse for a fair comparison (a missing or zero quarter since 2019, or fewer than three rising and three falling headings above the threshold) are left out until the data allows. In this version: Luxembourg.

| Field | Type | Meaning |
| --- | --- | --- |
| `country_code` | text | Importing member state, Comext reporter code (ISO 3166-1 alpha-2; Greece is `GR`) |
| `country` | text | Member state name in English |
| `section` | text | `total`, `largest chapter (HS2)`, `fastest-rising heading (HS4)`, `steepest-falling heading (HS4)` or `quarterly total` |
| `code` | text | HS2 chapter or HS4 heading code; empty for totals |
| `description` | text | Eurostat product label (chapter labels in sentence case) |
| `period` | text | Quarter the value covers, e.g. `Q2 2026` |
| `value_eur` | integer | Imports from China in that quarter, € |
| `previous_period` | text | Comparison quarter; empty for quarterly totals |
| `previous_value_eur` | integer | Imports in the comparison quarter, € |
| `change_pct` | number | % change, 1 decimal place |
| `eur_per_kg` | number | € per kg of net mass in the current quarter; empty for totals |

**Source:** Eurostat, Comext dataset DS-045409 (EU trade since 1988 by HS2-4-6 and CN8), Comext dissemination API: reporter = member state, partner = China (CN), flow = imports, indicators VALUE_IN_EUROS and QUANTITY_IN_100KG. <https://ec.europa.eu/eurostat/comext/newxtweb/>

**Acknowledgement and re-use:** Eurostat's copyright notice (<https://ec.europa.eu/eurostat/help/copyright-notice>) authorises re-use of its statistical data "for commercial or non-commercial purposes ... provided the source is acknowledged", and asks that modifications be stated clearly. Keep:

> Source: Eurostat (Comext, DS-045409). Quarterly totals, year-on-year changes, rankings and value per kg calculated from the monthly figures by Cambridge China Bridge.

## 4. China public holidays and factory closures

Live page: <https://camchinabridge.co.uk/tools/china-holiday-calendar/> (中文: <https://camchinabridge.co.uk/zh/tools/china-holiday-calendar/>)
Subscribable calendar: <https://camchinabridge.co.uk/china-holidays.ics>

One row per holiday period, make-up working day, or estimate:

- **2026, official:** every holiday period and make-up working day in the State Council General Office notice 国办发明电〔2025〕7号 (4 November 2025).
- **2027, provisional:** only the statutory days fixed by the national holiday regulations (全国年节及纪念日放假办法, as amended from 1 January 2025). The 2027 notice will add bridged days and make-up working days, so the real breaks will be longer. Qingming, Dragon Boat and Mid-Autumn 2027 are left out until the notice gives their dates.
- **Estimates:** Cambridge China Bridge's estimate of the 2027 pre-Chinese-New-Year factory rush and the real factory closure, which runs well beyond the statutory days. These are our judgement, not official dates, and are marked `status = estimate`.

Canton Fair dates on the live calendar are not included.

| Field | Type | Meaning |
| --- | --- | --- |
| `year` | year | Year of the holiday arrangement |
| `entry_type` | text | `public_holiday`, `make_up_workday`, `factory_rush_estimate` or `factory_closure_estimate` |
| `holiday_key` | text | Stable id: `newYear`, `springFestival`, `qingming`, `labourDay`, `dragonBoat`, `midAutumn`, `nationalDay` |
| `name_en` / `name_zh` | text | Name in English / Simplified Chinese |
| `start_date` / `end_date` | date | First and last day (inclusive), ISO 8601, China calendar date |
| `days` | integer | Calendar days, both ends included |
| `status` | text | `official`, `provisional_statutory` or `estimate` |
| `source_title` / `source_url` | text | The notice or page the dates come from |
| `checked_on` | date | When we last checked the source |

**Sources:**

- 国务院办公厅关于2026年部分节假日安排的通知（国办发明电〔2025〕7号）: <https://www.gov.cn/zhengce/content/202511/content_7047090.htm>
- 国务院关于修改《全国年节及纪念日放假办法》的决定: <https://www.gov.cn/zhengce/content/202411/content_6986380.htm>

## How it is updated

- **Import, Export and China–EU Pulses:** rebuilt each month from the HMRC and Eurostat APIs (Eurostat also publishes each month around the middle of the following month). HMRC publishes a month's figures around the middle of the following month, so the file changes when a new quarter is complete (roughly February, May, August and November) or when HMRC revises figures. Each refresh gets a new version number (`YYYY.MM`).
- **Holidays:** updated the day the State Council publishes the next year's notice (usually in the autumn), at which point the 2027 rows become `official` and gain make-up working days.

The live site is always the most current copy; this repository mirrors it.

## How to cite

> Cambridge China Bridge (2026). *UK-China Import Pulse*, Q2 2026 (version 2026.10.1). https://camchinabridge.co.uk/uk-china-import-pulse/. Contains HMRC data © Crown copyright 2026, licensed under the Open Government Licence v3.0.

> Cambridge China Bridge (2026). *UK-China Export Pulse*, Q2 2026 (version 2026.10.1). https://camchinabridge.co.uk/uk-china-export-pulse/. Contains HMRC data © Crown copyright 2026, licensed under the Open Government Licence v3.0.

> Cambridge China Bridge (2026). *China-EU Trade Pulse*, Q2 2026 (version 2026.10.1). https://camchinabridge.co.uk/china-eu-trade-pulse/. Source: Eurostat (Comext, DS-045409); quarterly figures calculated by Cambridge China Bridge.

> Cambridge China Bridge (2026). *China public holidays and factory closures* (version 2026.10.1). https://camchinabridge.co.uk/tools/china-holiday-calendar/. Official dates from State Council notices.

`CITATION.cff` gives the same in machine-readable form (GitHub shows a "Cite this repository" button from it).

## Licence

See `LICENSE`. In short:

- Our compilation, field design, bilingual names and the estimate rows: **CC BY 4.0**. Credit "Cambridge China Bridge" with a link to the page.
- HMRC-derived figures: **© Crown copyright, Open Government Licence v3.0**. Keep the OGL attribution above. Neither HMRC nor the Crown endorses Cambridge China Bridge or your use of the data.
- Eurostat-derived figures (China–EU Trade Pulse): re-use authorised provided the source is acknowledged ("Source: Eurostat"); our aggregation and calculations are stated above. Eurostat is not responsible for them.
- Official Chinese holiday dates are public facts from government notices; we claim no rights in them.

No warranty. Check official sources before relying on a date or figure for a contract or a shipment.

## Contact

Cambridge China Bridge, a trading name of Piggy Pineapple Ltd · <https://camchinabridge.co.uk/> · enquiries@camchinabridge.co.uk
