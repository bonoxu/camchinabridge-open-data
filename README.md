# Cambridge China Bridge open data

Two small, machine-readable datasets for UK importers, researchers, journalists and AI tools, published by [Cambridge China Bridge](https://camchinabridge.co.uk/), a China sourcing service based in Cambridge, UK.

| File | What it is | Rows | Licence |
| --- | --- | --- | --- |
| `data/uk-china-import-pulse.csv` | UK imports from China: latest quarter vs the same quarter a year earlier, by HS chapter and heading, plus quarterly totals since 2019 | 59 | Open Government Licence v3.0 (HMRC data, Crown copyright) |
| `data/china-holidays-factory-closures.csv` | China's public holidays, make-up working days (调休) and estimated Chinese New Year factory closures | 19 | CC BY 4.0 (compilation and estimates); official dates are public facts |

Both are described in `datapackage.json` ([Frictionless Data](https://specs.frictionlessdata.io/) Data Package), so tools such as `frictionless validate datapackage.json` can read the schema directly.

Version **2026.10** (Import Pulse covers Q2 2026 vs Q2 2025; holiday dates checked 27 September 2026).

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

## 2. China public holidays and factory closures

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

- **Import Pulse:** rebuilt each month from the HMRC API. HMRC publishes a month's figures around the middle of the following month, so the file changes when a new quarter is complete (roughly February, May, August and November) or when HMRC revises figures. Each refresh gets a new version number (`YYYY.MM`).
- **Holidays:** updated the day the State Council publishes the next year's notice (usually in the autumn), at which point the 2027 rows become `official` and gain make-up working days.

The live site is always the most current copy; this repository mirrors it.

## How to cite

> Cambridge China Bridge (2026). *UK-China Import Pulse*, Q2 2026 (version 2026.10). https://camchinabridge.co.uk/uk-china-import-pulse/. Contains HMRC data © Crown copyright 2026, licensed under the Open Government Licence v3.0.

> Cambridge China Bridge (2026). *China public holidays and factory closures* (version 2026.10). https://camchinabridge.co.uk/tools/china-holiday-calendar/. Official dates from State Council notices.

`CITATION.cff` gives the same in machine-readable form (GitHub shows a "Cite this repository" button from it).

## Licence

See `LICENSE`. In short:

- Our compilation, field design, bilingual names and the estimate rows: **CC BY 4.0**. Credit "Cambridge China Bridge" with a link to the page.
- HMRC-derived figures: **© Crown copyright, Open Government Licence v3.0**. Keep the OGL attribution above. Neither HMRC nor the Crown endorses Cambridge China Bridge or your use of the data.
- Official Chinese holiday dates are public facts from government notices; we claim no rights in them.

No warranty. Check official sources before relying on a date or figure for a contract or a shipment.

## Contact

Cambridge China Bridge, a trading name of Piggy Pineapple Ltd · <https://camchinabridge.co.uk/> · enquiries@camchinabridge.co.uk
