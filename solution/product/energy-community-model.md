# The energy-community model

> Seeded 2026-11-13 from the `v2` repo (`Dictionary.md`, `PriceOracle.md`, infrastructure matrix, README of
> `Hypha.Energy`). **R** unless noted. The v2 documents describe the target design; what is built today is
> **U** until checked against `Hypha.Energy`. The Dictionary is the source for exact terms.

## Concepts

- **Energy Community**: members who produce, store and consume together, with shared assets (solar park, battery).
- **Aggregator / Orchestrator**: the operator entity facilitating flows. On-chain, the Aggregator collects a fee
  (`aggregatorFeeBps`, basis points of LOCAL revenue) each settlement interval; the community treasury collects its own.
  (Website voice says **Orchestrator** for the local operator role; the Dictionary says **Aggregator** for the on-chain
  fee role. Check whether they are the same entity before merging the terms. **U**)
- **Interval**: 15 minutes. Each interval gets a monetary value per energy flow.
- **Ownership**: shares of an asset in basis points (10,000 = 100%); battery ownership is separate from solar.
- **Coverage Ratio (rho)**: community production over consumption. Surplus when rho >= 1, deficit when rho < 1.

## Pricing: the Delphi price oracle

Three roles: **Bridge** (connects data), **Balancer** (shifts prices to encourage consumption in surplus and
conservation in deficit), **Democratizer** (DAO-voted coefficients).

- **Algorithmic engine** (community flows, "Cost Price Plus"): `P_eff = P_base - alpha*(rho - 1)` in surplus,
  `P_eff = P_base + beta*(1 - rho)` in deficit; `P_base`, alpha, beta set by DAO vote.
- **Market pricing** (retailer flows): day-ahead prices plus retailer markups and loss profiles (`Price_Engine`).
- Battery prices include a storage premium for wear and losses; battery is controlled only by the EMS.

## Settlement

Fair-split algorithm distributes each interval's revenue to source owners after fees; the design requires a
**zero-sum invariant** across interval accounting. Value settles on-chain through energy-source tokens; the
Hypha platform holds governance and treasury.

## Tokens (Hypha Energy Space)

EPARTS (ownership), EVOICE (voice), ECREDITS, EURCREDITS. Hypha token also held by members. **S/R** from the
2026-11-09 holders export; meanings of the credit tokens to be confirmed with the founders. **U**

## Where to read more

`Hypha-Energy/v2` (public): `Dictionary.md`, `PriceOracle.md`, `simulator-hypha-mapping-v1.md`,
`hypha-energy-user-metrics-api.md`. `Hypha.Energy/docs/design/price-and-settlement.md`.
