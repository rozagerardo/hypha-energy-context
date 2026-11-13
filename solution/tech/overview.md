# Tech overview

> Seeded 2026-11-13 from READMEs and `v2` docs (**R**); not yet verified by running anything (**U** unless noted).
> The `v2` repo is a plan ("POC Azure/C#"); the `Hypha.Energy` monorepo is what exists.

## Layers and data flow

```
edge meters (zigbee2mqtt gateway) --MQTT (HiveMQ / EMQX)--> ingestion worker --> TimescaleDB (raw_readings)
                                                                                      |
 day-ahead prices --> Price_Engine / PriceEngine (retail import/export price) --> oracle.* tables
                                                                                      |
 Hypha_forecast (ARIMA: solar, demand)  --writes forecast.* -->                       |
 Hypha_EMS / Hypha.EMS (dispatch: battery, EV, solar) <-- prices, forecasts, readings |
                                                                                      v
              Hypha.API (REST; sole writer of EMS control and community/user data)  --> Hypha.EMOS (Blazor Server UI)
                                                                                      |
                      fair-split settlement per 15 min --> on-chain (EnergySourceToken, Hypha platform tokens)
```

- **Edge to database** (Zek's scope in the v2 plan): meters over MQTT at about 10 s heartbeat, ingestion
  worker (C#, Azure WebJobs), TimescaleDB as the integration/handover point. Azure IoT Hub kept parked.
- **Price oracle** ("Delphi"): assigns a price to each 15-minute interval. Algorithmic engine uses a
  Coverage Ratio with DAO-voted coefficients; retailer flows use market pricing. See `../product/energy-community-model.md`.
- **EMS**: rule-based dispatch worker in the monorepo (`Hypha.EMS`); a Python LP optimiser prototype
  (`Hypha_EMS`, linopy) and an MSc thesis controller (24 h planning on top of EMS and settlement).
- **Forecast**: demo ARIMA service on Azure Container Apps (not hardened).
- **Anomaly detection**: separate Python service (XGBoost, FastAPI, Streamlit) on solar assets.
- **Hosting**: Azure (App Service, Container Apps, PostgreSQL). Websites deploy through GitHub Actions to Azure App Service (website) or by hand (value-x).

## Bridge to the Hypha platform

Governance, ownership and treasury live on the Hypha platform (`hypha-web`, Hypha DAO). The Hypha
platform's Energy tab (`/dho/<slug>/energy`) shows community energy data; the **white-label iframe embed**
presents a Space on a partner site (read-only, public Spaces): the basis of agreement Block 1.
Tracks: `C:\Users\rozag\projects\hypha\src\hypha-integration-kit-context\tracks\white-label.md`.

## Unknowns worth resolving

- How `Hypha.Energy` data reaches the Hypha platform Energy tab (API v2 endpoints? DB?). **U**
- Which parts of the v2 plan (on-chain settlement, EnergySourceToken) are built vs planned. **U**
- Environments and who holds access (Azure, MQTT brokers, DBs). **U**
