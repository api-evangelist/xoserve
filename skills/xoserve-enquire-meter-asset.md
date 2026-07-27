---
name: Enquire on a gas meter asset (v1 and v2)
description: >-
  Retrieve meter asset detail for a GB gas meter point by MPRN and/or meter serial number using the
  Xoserve Meter Asset API, and choose correctly between v1 (16 fields, snapshot) and v2 (42 fields,
  with supplier / MAP / MAM history). Access administered by RECCo under the Retail Energy Code.
api: openapi/xoserve-meter-asset-api-v2-openapi.yml
operations: [getMeterAsset_1]
---

# Enquire on a gas meter asset

## Before you start

Meter Asset Enquiry is a Gas Enquiry Service product under the Retail Energy Code, governed by RECCo.
Apply via `enquiries@recmanager.co.uk`, not to Xoserve. `subscriptionRequired: true` and
`approvalRequired: true`.

## Pick your version first

Both versions are live at the same time, share one APIM version set, and expose the **same
operationId `getMeterAsset_1` with the same two parameters**. They differ only in the response.

| | v1 | v2 |
|---|---|---|
| Base URL | `https://discoveryapi.correla.com/meter-asset/v1` | `https://discoveryapi.correla.com/meter-asset/v2` |
| Response fields | 16 | 42 |
| Spec | `openapi/xoserve-meter-asset-api-v1-openapi.yml` | `openapi/xoserve-meter-asset-api-v2-openapi.yml` |
| Schema | `json-schema/xoserve-meter-asset-api-v1-components.json` | `json-schema/xoserve-meter-asset-api-v2-components.json` |

**Default to v2.** Xoserve describes it as "contains additional output fields when compared to v1", and
v2 is the version bundled into the current Meter Asset Enquiry product. Use v1 only if your
subscription is pinned to it. Xoserve publishes no deprecation notice or sunset date for v1 — see
`lifecycle/xoserve-lifecycle.yml`.

The `/v1` or `/v2` segment is **mandatory**; the exported OpenAPI `servers` entry omits it and the
versionless path returns `404 Resource not found`.

## Authentication

```
APIKey: <your subscription key>
```

## Call the operation

Operation: **`getMeterAsset_1`** — `GET /`.

| Parameter | Type | Meaning |
|---|---|---|
| `mprn` | integer (int64) | Meter Point Reference Number — "a unique identifier for the point at which a meter is, has been or will be connected to the Gas Network" |
| `msn` | string | "The manufacturer's meter serial number as held on the physical meter currently installed on the supply point" |

```
GET https://discoveryapi.correla.com/meter-asset/v2?mprn=1234567890&msn=E6S00123456789
APIKey: <key>
Accept: application/json
```

`application/xml` is also published, returning the same schema under an XML root element.

## Read the response

`MeterAssetRestResponse` wraps a `MeterAsset` object under the `meter` key. Example:
`examples/xoserve-meter-asset-api-v2-response-example.json`.

**v1 (16 fields)** — the asset snapshot: `mprn`, `msn`, `current_supplier`, `current_supplier_name`,
`meter_capacity`, `meter_mechanism`, `meter_type`, `meter_model`, `meter_year_of_manufacture`,
`smp_status`, `meter_installation_date`, `meter_asset_manager_effective_date`,
`meter_asset_manager_id`, `meter_asset_manager_name`, `service_effective_valid_from`, `map_id`.

**v2 adds 26 fields** — history and smart-metering status:

- Asset: `meter_asset_manufacturer`, `removal_date`.
- Smart / DCC: `dcc_service_flag`, `dcc_service_flag_effective_from_date`.
- Network: `network_short_code`, `gt_reference_number`.
- Party history: `current_shipper`, `incoming_supplier`, `incoming_supplier_short_code`,
  `incoming_supplier_effective_from_date`, `previous_supplier`, `previous_supplier_short_code`,
  `previous_supplier_effective_from_date`, `previous_supplier_effective_to_date`,
  `current_supplier_effective_date`.
- MAP history: `current_map_effective_from_date`, `current_map_effective_to_date`,
  `previous_map_short_code`, `previous_map_effective_from_date`, `previous_map_effective_to_date`,
  `incoming_map_short_code`, `incoming_map_effective_from_date`.
- MAM history: `previous_mam`, `previous_mam_short_code`, `previous_mam_effective_from_date`,
  `previous_mam_effective_to_date`.

If your job is data assurance on meter asset records, v1 is enough. If your job is reconciling a
switch or a MAP/MAM change, you need v2.

## Errors, retries, budget

Azure APIM envelope `{"statusCode", "message"}`, not problem+json. `401` missing/invalid key, `404`
missing version segment. `GET`, so safe and idempotent — retry freely on transport failure, but every
call is metered and there is no remaining-quota header. See `errors/xoserve-problem-types.yml` and
`rate-limits/xoserve-rate-limits.yml`.

## Privacy

Meter serial plus MPRN plus supplier history identifies a specific premise and its switching history.
Query only MPRNs already in scope for your regulated purpose, and audit each query
(`agentic-access/xoserve-agentic-access.yml`).
