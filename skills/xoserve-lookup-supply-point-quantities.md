---
name: Look up Formula Year AQ and SOQ for a gas supply point
description: >-
  Retrieve the current and proposed Formula Year Annual Quantity (AQ) and Supply Offtake Quantity
  (SOQ) values for a GB gas meter point, by MPRN or by postcode with address detail, using the
  Xoserve Shipper API (Supply Point Quantities). Gas Shippers only.
api: openapi/xoserve-shipper-api-openapi.yml
operations: [getShipper_1]
---

# Look up Formula Year AQ and SOQ for a gas supply point

## Before you start — this API is not open

You cannot call this API without being a **licensed GB gas Shipper** with an approved subscription.
Xoserve's portal reports `subscriptionRequired: true` **and** `approvalRequired: true` on the Supply
Point Quantities product. Keys are issued only after a subscription contract is countersigned; apply
via `xoserve.customer.lifecycle.team@xoserve.co.uk`. An unauthenticated call returns `401 Access
denied due to missing subscription key`. There is no consumer-consent route into this API — do not
attempt to use it on behalf of a gas consumer.

## Authentication

Send the Azure API Management subscription key as a request header:

```
APIKey: <your subscription key>
```

The key may alternatively be passed as a `subscription-key` query parameter, but prefer the header so
the secret does not land in logs or referrers. A key is bound to one subscription and cannot be reused
across subscriptions. See `authentication/xoserve-authentication.yml`.

## Base URL

```
https://discoveryapi.correla.com/shipper/v1
```

The **`/v1` segment is mandatory** even though the provider's exported OpenAPI `servers` entry omits
it. Calling `https://discoveryapi.correla.com/shipper` returns `404 Resource not found`.

## Step 1 — choose a filter

Operation: **`getShipper_1`** — `GET /`.

At least one filter is mandatory. The provider's own wording: *"There are two different filters that
could be applied to the API call, where at least 1 is mandatory. These is MPRN, or POSTCODE, the latter
could be combined with additional address details."*

| Route | Parameters |
|---|---|
| By meter point | `mprn` (int64) |
| By address | `postcode`, optionally narrowed with `house_no`, `street`, `town`, `county`, `country`, `sub_building_name`, `dependent_street`, `dependent_local` |

Prefer `mprn` when you have it. Postcode lookup is address-space enumerable, so only use it when you
are resolving a specific premise you already have a regulated purpose for.

## Step 2 — call the operation

```
GET https://discoveryapi.correla.com/shipper/v1?mprn=1234567890
APIKey: <key>
Accept: application/json
```

Both `application/json` and `application/xml` representations are published; `application/xml` returns
the same schema with an XML root element.

## Step 3 — read the response

The payload is a `ShipperRestResponse` wrapping a `ShipperResponse` under the `mprn` key (24 fields).
See `examples/xoserve-shipper-api-response-example.json` and
`json-schema/xoserve-shipper-api-components.json`. The values that matter:

- `current_formula_year_aq_value`, `current_formula_year_soq_value` — the values in force now.
- `perspective_formula_year_aq_value`, `perspective_formula_year_soq_value`,
  `perspective_formula_year_effective_date` — the **proposed** values for the coming Formula Year.
  This is the whole point of the API: seeing them before the late-March notification to the current
  Shipper.
- `current_aq_roll_value`, `current_soq_roll_value` — rolling values.
- Network placement: `local_distribution_zone`, `exit_zone`, `distribution_network_operator`,
  `market_sector_code`, `csep_id`, `amr_indicator`, `installation_number`.
- Address echo: `house_no`, `street`, `town`, `county`, `country`, `postcode`, `sub_building_name`,
  `dependent_street`, `dependent_local`.

## Errors

There is no error reference and the provider documents only the 200 response. The envelope is the
Azure APIM shape `{"statusCode": <int>, "message": "<string>"}` — **not** RFC 9457 problem+json.

| Status | Meaning | Do this |
|---|---|---|
| 401 missing subscription key | No `APIKey` header / `subscription-key` param | Add the header |
| 401 invalid subscription key | Wrong, revoked, or cross-subscription key | Re-check the key against the subscription |
| 404 Resource not found | Version segment omitted, or gateway root called | Include `/v1` |

Full detail: `errors/xoserve-problem-types.yml`.

## Retries and budget

`getShipper_1` is a `GET` — safe and idempotent, so a transport-level retry can never duplicate an
effect. There is **no** `Idempotency-Key` header on this API and none is needed.

But **every call, including a retry, consumes annual allowance.** Xoserve meters this API by an annual
call band written into your contract — A (60,000/year) through F (18,000,000/year). Nothing in the
response tells you how much is left, and there is no 429. Track consumption locally and cap agent
retries. See `rate-limits/xoserve-rate-limits.yml`.

## Handling and privacy

Every response identifies a real GB premise. Do not enumerate postcodes, do not cache beyond the
regulated purpose you obtained the data for, and log the purpose alongside every MPRN queried — see
`agentic-access/xoserve-agentic-access.yml` (`subject: required`, `purpose-required: true`,
`audit: required`).
