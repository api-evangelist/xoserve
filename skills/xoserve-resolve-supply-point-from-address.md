---
name: Resolve a gas supply meter point from an address
description: >-
  Resolve a GB postal address, MPRN or address_id to a full supply meter point record — current and
  previous supplier, network, meter, latest read and quantity data — using the Xoserve Supplier API
  (Supply Point Enquiry). Access administered by RECCo under the Retail Energy Code.
api: openapi/xoserve-supplier-api-openapi.yml
operations: [getSupplier_1]
---

# Resolve a gas supply meter point from an address

## Before you start — access comes from RECCo, not Xoserve

The Supply Point Enquiry API is part of the Gas Enquiry Service (GES), provided under the Retail
Energy Code and governed by the Retail Energy Code Company. **Do not apply to Xoserve for this one.**
Contact `enquiries@recmanager.co.uk`; eligibility is subject to the REC Data Access Matrix and is
administered through the REC Portal (`https://recportal.co.uk/`), including an annual GES password
reset process. The portal product carries `subscriptionRequired: true` and `approvalRequired: true`.

## Authentication

```
APIKey: <your subscription key>
```

Azure API Management subscription key, header form preferred over the `subscription-key` query
parameter. See `authentication/xoserve-authentication.yml`.

## Base URL

```
https://discoveryapi.correla.com/supplier/v1
```

The `/v1` segment is mandatory; the exported OpenAPI `servers` entry omits it and the versionless path
404s.

## Step 1 — pick one of the three filter routes

Operation: **`getSupplier_1`** — `GET /`.

Provider wording: *"There are 3 different filters that could be applied to the API call, where at least
1 is mandatory. These are mprn, address_id or postcode, the latter could be combined with additional
address detail."*

| Route | Parameters |
|---|---|
| By meter point | `mprn` (int64) |
| By internal address key | `address_id` (int64) — the BDP ID linking MPRN to address data |
| By address | `postcode`, narrowed with `house_no`, `street`, `Town`, `county`, `country`, `sub_building_name`, `dependent_street`, `dependent_local` |

> **Gotcha — capital T.** On this API the town filter is spelled **`Town`**, not `town` as on the
> Shipper API. This is the provider's own spelling; reproduce it exactly.

## Step 2 — call the operation

```
GET https://discoveryapi.correla.com/supplier/v1?postcode=XX00%200XX&house_no=1&Town=Solihull
APIKey: <key>
Accept: application/json
```

## Step 3 — read the response

`SupplierRestResponse` wraps a `SupplierFull` object under the `supplier` key — **89 fields**, the
richest record in the estate. Schema: `json-schema/xoserve-supplier-api-components.json`. Example:
`examples/xoserve-supplier-api-response-example.json`. Field groups worth knowing:

- **Identity / address** — `mprn`, `address_id`, `uprn`, and the full PAF-shaped address set
  (`house_name`, `house_no`, `po_box_no`, `sub_building_name`, `dependent_street`, `dependent_local`,
  `double_dependent_local`, `delivery_point_alias`, `street`, `town`, `county`, `country`, `postcode`).
- **Switching state** — `current_supplier`, `current_supplier_name`, `incoming_supplier`,
  `previous_supplier_name`, `previous_supplier_short_code`, `supply_point_withdrawal_status`,
  `confirmation_reference_number`, `confirmation_effective_date`, `shipper_name`, `shipper_short_code`.
- **Network** — `network_name`, `network_owner_effective_from_date`, `ldz_id`, `exit_zone`,
  `csep_id`, `csep_max_annual_quantity`, `csep_supply_point_offtake_quantity`, IGT charging fields.
- **Meter** — `msn`, `meter_capacity`, `meter_mechanism`, `meter_type`, `meter_model`,
  `meter_manufacturer`, `meter_year_of_manufacture`, `meter_installation_date`, `meter_location`,
  `meter_units`, `meter_number_of_dials`, `meter_imperial_indicator`, `meter_device_status`,
  `correction_factor`, `mam_short_code`, `gas_act_owner`.
- **Smart / DCC** — `smso_id`, `sms_operating_entity_efd`, `dcc_service_flag`, `dcc_service_flag_efd`,
  `first_smets_installation_date`, `ihd_install_status`, `amr_indicator`, `amr_sp`.
- **Reads and quantities** — `latest_meter_read_date`, `latest_meter_read_type`,
  `latest_meter_read_value`, `meter_read_batch_frequency`, `annual_quantity`,
  `formula_year_annual_quantity`, `formula_year_offtake_quantity`, `offtake_quantity`,
  `smp_prospective_formula_year_aq`, `smp_prospective_formula_year_soq`,
  `smp_prospective_formula_year_effective_date`.
- **Classification** — `class`, `market_sector_code`, `end_user_category_code`,
  `end_user_category_identifier`, `small_large_supply_point_indicator`, `daily_metered_indicator`,
  `priority_consumers_indicator`, `interruption_contract_exists`, `isolation_status`.

Only the **latest** meter read is exposed — there is no read series and no consumption history
endpoint. If you need meter asset detail alone, use the Meter Asset skill instead: it is the cheaper,
narrower call.

## Errors

Azure APIM envelope `{"statusCode", "message"}`; not problem+json. `401` for a missing or invalid key,
`404` when the `/v1` segment is dropped. Only the 200 response is documented by the provider — expect
undocumented backend behaviour for an unknown MPRN or a filter combination that matches nothing, and
code defensively for an empty or partial `supplier` object. See `errors/xoserve-problem-types.yml`.

## Retries, pagination and budget

`GET`, therefore safe and idempotent; no idempotency key exists or is needed. **There is no
pagination** — a postcode lookup returns a single object, not a list, so do not build cursor logic.
Charging sits in the REC Charging Statement rather than in Xoserve's call bands, but every call is
still metered; cap agent retries.

## Privacy

`priority_consumers_indicator` flags vulnerable consumers, and the record ties an address to a named
supplier and consumption. Treat every response as personal-data-adjacent under UK GDPR. Never resolve
addresses speculatively; record the regulated purpose with each query
(`agentic-access/xoserve-agentic-access.yml`).
