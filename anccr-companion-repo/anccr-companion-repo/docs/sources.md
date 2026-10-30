# Sources

The sample imitates a real NAVFAC export so the Data Drop Zone, Bill Sentinel and Forecaster can be exercised before live data arrives. Each source is either the schema being imitated or the public feed the platform reads once a key is supplied.

| Source | What it supplies | Link |
|---|---|---|
| EDW / BIMS utility invoice export | The 19-column line-item schema the generator writes | Internal NAVFAC source; imitated, not accessed |
| NOAA NCEI Climate Data Online, GSOM | Heating and cooling degree-days by month (station GHCND:USW00013737, Norfolk Intl) | https://www.ncei.noaa.gov/cdo-web/ |
| EIA Open Data API v2 | Monthly commercial retail electricity price by state | https://www.eia.gov/opendata/ |
| EIA Electric Power Monthly (EIA-861M) | Published tables behind the API | https://www.eia.gov/electricity/monthly/ |
| OpenEI Utility Rate Database | Commercial rate schedules that map onto the Tariff Engine | https://openei.org/wiki/Utility_Rate_Database |
| Dominion Energy Virginia | General Service schedule structure (GS-2, GS-3, GS-4) | https://www.dominionenergy.com/virginia/rates-and-tariffs |
| Virginia Natural Gas | Gas General Service structure | https://www.virginianaturalgas.com/ |
| City of Norfolk Department of Utilities | Water volumetric rate and service fee | https://www.norfolk.gov/utilities |
| HRSD | Wastewater treatment rate billed through HRUBS | https://www.hrsd.com/ |
| Virginia State Corporation Commission | Where Dominion and VNG rate cases are docketed | https://www.scc.virginia.gov/ |

## Sample dataset at a glance

- Installation: Naval Station Norfolk (one site)
- Accounts: 10 across electric, gas, water and sewer
- Period: January 2024 to June 2026 (30 months)
- Generator: seeded pseudo-random (seed 160), reproducible
- Planted errors: 6 (duplicate invoice, demand spike, meter multiplier, misapplied rate class, skipped month, catch-up bill)
- The Yorktown three-year validation set replaces this sample.
