# Data dictionary (sample tables)

Tables behind the Data pills in the companion. Types, units and descriptions are the ones the production data dictionary will carry; rows are synthetic.

## EDW / BIMS utility invoice line items

- Key: `edw`
- Source: NAVFAC EDW (imitated by the sample generator)
- Referenced by pills: Loaded bills, Line-item bills, EDW/BIMS line items, EDW query output, TDSX / Hyper extract, Interim Tableau/EDW feed
- Note: One row per line item; a bill is several rows sharing a bill number.

| field | type | unit | description |
|---|---|---|---|
| Installation | text | — | Installation name as held in Maximo |
| Complex Name | text | — | Sub-complex within the installation |
| Vendor | text | — | Billing utility |
| Account Number | text | — | Utility account |
| Bill Number | text | — | Invoice identifier |
| Status | code | — | APPR = approved for payment |
| Service End Date | date | — | DD-MON-YY |
| Spid | text | — | Service point id |
| Billing Description | text | — | Line item label |
| Unit Of Measure | code | — | KWH, KW, CCF, KGAL, TARIFF |
| Consumption | number | unit of measure | Metered quantity |
| Cost Usd | number | USD | Billed amount |
| Commodity | code | — | EL, NG, FW, SE |

## Account billing history

- Key: `history`
- Source: Derived from line items (ACR-08 → ACR-03)
- Referenced by pills: 3+ years history
- Note: The series the forecaster fits. Three or more years are needed for a stable seasonal fit.

| field | type | unit | description |
|---|---|---|---|
| account | text | — | Utility account |
| month | month | — | YYYY-MM service month |
| units | number | kWh | Billed consumption |
| cost | number | USD | Bill total |
| rate | number | USD/kWh | cost ÷ units |
| kw | number | kW | Billed demand |

## AMI interval data, 30-minute

- Key: `ami`
- Source: Meter data management export (imitated)
- Referenced by pills: AMI interval kWh, AMI 30-minute kWh
- Note: 48 interval columns per meter-day; six are shown. Two meters sit on the main-base account.

| field | type | unit | description |
|---|---|---|---|
| meter_id | text | — | AMI meter |
| date | date | — | Read date |
| 12:30 AM kWH | number | kWh | Interval energy |
| 1:00 AM kWH | number | kWh | Interval energy |
| … |  | — | 44 more intervals |
| 11:30 PM kWH | number | kWh | Interval energy |
| 12:00 AM kWH | number | kWh | Interval energy |
| day_total | number | kWh | Sum of 48 intervals |

## Tariff library

- Key: `tariffs`
- Source: UTRMS tariffs; URDB imports; pasted schedules
- Referenced by pills: Tariff library, UTRMS tariffs
- Note: Each schedule is stored as components so any account can be re-rated.

| field | type | unit | description |
|---|---|---|---|
| tariff_id | text | — | Library key |
| utility | text | — | Issuing utility |
| schedule | text | — | Published name |
| customer_charge | number | USD | Fixed monthly |
| energy_blocks | text | — | Block boundaries and rates |
| demand | number | USD/kW | Demand charge |
| ratchet | number | % | Share of 12-month peak |
| effective | date | — | Effective date |
| source | code | — | UTRMS, URDB, PASTE |

## OpenEI Utility Rate Database schedules

- Key: `urdb`
- Source: api.openei.org/utility_rates (commercial sector)
- Referenced by pills: OpenEI URDB
- Note: URDB tiers map directly onto the tariff engine blocks and demand charge.

| field | type | unit | description |
|---|---|---|---|
| label | text | — | URDB record id |
| utility | text | — | Utility name in URDB |
| name | text | — | Rate name |
| sector | code | — | Commercial / Industrial |
| fixedchargefirstmeter | number | USD/month | Fixed charge |
| energyratestructure | text | — | Tier summary |
| demandratestructure | text | — | Demand summary |
| startdate | date | — | Effective |

## Rate-case register (proposed schema)

- Key: `ratecases`
- Source: Not yet in any system — offered to the rate-intervention team
- Referenced by pills: Rate-case register
- Note: Deferred until an authoritative source is agreed. This is the shape the tariff engine would consume.

| field | type | unit | description |
|---|---|---|---|
| utility | text | — | Filing utility |
| docket | text | — | Commission case number |
| filed | date | — | Filing date |
| requested_pct | number | % | Requested change |
| status | code | — | Filed, Pending, Approved, Withdrawn |
| effective | date | — | Effective date if approved |
| affects | text | — | Schedules affected |
| owner | text | — | Navy tracker |

## NOAA degree-days (GSOM)

- Key: `noaa`
- Source: ncei.noaa.gov · station GHCND:USW00013737 (Norfolk Intl)
- Referenced by pills: NOAA degree-days
- Note: Heating and cooling degree-days by month, with 30-year normals, drive weather normalisation.

| field | type | unit | description |
|---|---|---|---|
| site | text | — | Installation |
| month | month | — | YYYY-MM |
| hdd | number | °F-days | Heating degree-days |
| cdd | number | °F-days | Cooling degree-days |
| normal_hdd | number | °F-days | 30-year normal |
| normal_cdd | number | °F-days | 30-year normal |

## EIA retail price benchmarks

- Key: `eia`
- Source: api.eia.gov v2 electricity/retail-sales (sector COM)
- Referenced by pills: EIA benchmarks, EIA state prices
- Note: Modeled until an import replaces it. The benchmark an installation’s effective rate is compared with.

| field | type | unit | description |
|---|---|---|---|
| state | code | — | Two-letter state |
| month | month | — | YYYY-MM |
| commodity | code | — | ELEC or GAS |
| sector | code | — | COM = commercial |
| price | number | USD/kWh or USD/Mcf | Average retail price |
| source | code | — | modeled / EIA-861M / API |

## Forecast runs

- Key: `forecast`
- Source: ACR-03 output, written to the audit trail
- Referenced by pills: Forecast runs
- Note: Each run stores its inputs so a band can be reproduced later.

| field | type | unit | description |
|---|---|---|---|
| run_id | text | — | Run identifier |
| account | text | — | Utility account |
| created | date | — | Run time |
| horizon | number | months | Months ahead |
| paths | number | — | Monte Carlo paths |
| fuel_drift | number | %/yr | Scenario driver |
| p10_rate | number | USD/kWh | 10th percentile, month 12 |
| p50_rate | number | USD/kWh | Median, month 12 |
| p90_rate | number | USD/kWh | 90th percentile, month 12 |

## Savings ledger

- Key: `ledger`
- Source: ACR-10 ledger of record
- Referenced by pills: Savings ledger
- Note: Validated savings and recoveries, each with the module and reference that produced it.

| field | type | unit | description |
|---|---|---|---|
| entry_id | text | — | Ledger entry |
| date | date | — | Posted |
| type | code | — | recovery, avoidance, tariff, procurement |
| source_module | code | — | ACR code |
| account | text | — | Utility account |
| amount | number | USD | Validated value |
| reference | text | — | Bill, case or memo id |
| status | code | — | open, validated, closed |

## Source allow-list (v1.9)

- Key: `allowlist`
- Source: Settings → governance
- Referenced by pills: Source allow-list
- Note: The analyst may draw on these sources and no others. Visible to every user.

| field | type | unit | description |
|---|---|---|---|
| source | text | — | Reference |
| kind | code | — | regulator, vendor, federal, market, weather |
| domain | text | — | Host the hub may read |
| allowed | code | — | yes / no |
| owner | text | — | Who approves changes |

## Forward market curves

- Key: `curves`
- Source: S&P Global / EIA (import-first)
- Referenced by pills: Market curves (S&P / EIA)
- Note: Monthly forward prices at the delivery hub; the strip behind the IGE.

| field | type | unit | description |
|---|---|---|---|
| hub | text | — | Trading hub |
| delivery | month | — | Delivery month |
| price | number | USD/MWh | Forward price |
| as_of | date | — | Curve date |
| source | code | — | S&P, EIA, broker |

## Supply contracts

- Key: `contracts`
- Source: Contracting records (imitated)
- Referenced by pills: Contract terms
- Note: Terms the procurement workbench compares against the forward curve.

| field | type | unit | description |
|---|---|---|---|
| contract | text | — | Contract id |
| supplier | text | — | Supplier |
| structure | code | — | fixed, index, blended |
| start | date | — | Start |
| end | date | — | End |
| volume | number | MWh | Annual volume |
| fixed_price | number | USD/MWh | Fixed component |
| index_share | number | % | Share on index |

## Demand-response programs

- Key: `programs`
- Source: Utility and RTO program tariffs
- Referenced by pills: Program tariffs
- Note: Event windows and payments used to value curtailment.

| field | type | unit | description |
|---|---|---|---|
| program | text | — | Program |
| operator | text | — | Utility or RTO |
| season | code | — | summer, winter, annual |
| event_window | text | — | Hours events may be called |
| capacity_pay | number | USD/kW-month | Capacity payment |
| energy_pay | number | USD/kWh | Energy payment |
| max_events | number | — | Per season |

## Business-case inputs

- Key: `bcase`
- Source: ACR-02, ACR-03 and ACR-05 results
- Referenced by pills: Outputs of ACR-02 / 03 / 05
- Note: What the builder pulls in before writing the memo.

| field | type | unit | description |
|---|---|---|---|
| opportunity | text | — | Opportunity |
| source_module | code | — | ACR code |
| baseline_cost | number | USD | Current annual cost |
| proposed_cost | number | USD | Proposed annual cost |
| p50_saving | number | USD | Median saving |
| p10_saving | number | USD | Downside |
| recommendation | text | — | Draft recommendation |

## Vendor master

- Key: `vendors`
- Source: ACR-09 utility registry
- Referenced by pills: Vendor master
- Note: One row per utility partner.

| field | type | unit | description |
|---|---|---|---|
| vendor_id | text | — | Registry key |
| name | text | — | Utility |
| hq | text | — | Headquarters |
| state | code | — | Service state |
| commodity | code | — | ELEC, GAS, WATER, SEWER |
| accounts | number | — | Accounts billed |
| contact | text | — | Account manager |

## Permission matrix (proposed)

- Key: `perms`
- Source: Settings → access; enforced by the host or relay
- Referenced by pills: Permission matrix
- Note: Rows are personas; columns are modules and the data grain each may see.

| field | type | unit | description |
|---|---|---|---|
| persona | text | — | Role |
| portfolio | code | — | grain |
| sentinel | code | — | grain |
| tariffs | code | — | grain |
| forecast | code | — | grain |
| procurement | code | — | grain |
| ledger | code | — | grain |
| settings | code | — | access |

## External feeds

- Key: `feeds`
- Source: ACR-08 feed panel
- Referenced by pills: External feeds
- Note: Import-first today; live fetch when the network allows and a key is supplied.

| field | type | unit | description |
|---|---|---|---|
| feed | text | — | Feed |
| endpoint | text | — | API or portal |
| key_required | code | — | yes / no |
| schema | text | — | Columns accepted |
| status | code | — | modeled, imported, live |
| used_by | text | — | Modules |

## Tariff assignment check

- Key: `assign`
- Source: ACR-02 best fit against the billed tariff
- Referenced by pills: Correct tariff assignment
- Note: The single check that validates the engine: does best fit equal what the FEC is billed under?

| field | type | unit | description |
|---|---|---|---|
| account | text | — | Utility account |
| billed_tariff | text | — | From the invoice |
| best_fit | text | — | Engine ranking #1 |
| runner_up | text | — | Engine ranking #2 |
| delta_12m | number | USD | Best fit minus runner-up |
| match | code | — | yes / no |

