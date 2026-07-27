# Xoserve (xoserve)

Xoserve Limited is the Central Data Services Provider (CDSP) for Great Britain's gas market — funded, governed and owned by the gas industry itself. It operates the central register of every gas supply point in Britain and runs the systems the market clears through: UK Link, the Gemini transmission system, the Gas Enquiry Service, the Contact Management Service, SwitchStream and the Data Discovery Platform. It sits between the gas transporters, the independent gas transporters, the shippers and the retail suppliers under the Uniform Network Code (UNC) and the Data Services Contract (DSC), with further obligations under the Retail Energy Code. Britain mandated this data infrastructure, not a consumer data right: there is no CDR-style, consent-driven data portability obligation on Xoserve, and no Green Button or Consumer Data Standards implementation. Xoserve does publish a real developer portal — the Discovery API Platform, delivered by its service provider Correla — where four documented REST APIs expose meter-point-level information, browsable anonymously but callable only by eligible, contracted GB gas industry parties.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/xoserve/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/xoserve/refs/heads/main/apis.yml)

## Tags

- Energy
- United Kingdom
- Gas
- Utilities
- Energy Markets
- Meter Data
- Gas Networks
- Central Data Service Provider
- Data Services

## Timestamps

- **Created:** 2026-07-27
- **Modified:** 2026-07-27

## APIs

### Xoserve Shipper API

Bundled by Xoserve as the Supply Point Quantities service. Lets gas Shippers view the proposed Formula Year (Billing) Annual Quantity value for a meter point ahead of the late-March notification to the current Shipper, together with meter point and network information. A single GET operation filtered by MPRN, or by postcode combined with optional address detail. Returns JSON or XML. Available to Gas Shippers only, after contract countersigning.

- **Human URL:** [https://discoveryapiportal.correla.com/api-details#api=shipper&operation=getShipper_1](https://discoveryapiportal.correla.com/api-details#api=shipper&operation=getShipper_1)
- **Base URL:** `https://discoveryapi.correla.com/shipper/v1`

#### Tags

- Gas
- Supply Point
- Annual Quantity
- Energy Markets

#### Properties

- [OpenAPI](openapi/xoserve-shipper-api-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [API Reference](https://discoveryapiportal.correla.com/api-details#api=shipper&operation=getShipper_1)
- [Documentation](https://www.xoserve.com/products-services/data-products/gas-apis/)
- [Portal](https://discoveryapiportal.correla.com/)
- [Sign Up](https://discoveryapiportal.correla.com/signin)
- [Pricing](https://www.xoserve.com/media/7975/xoserve-try-before-you-buy-api-service-subscription-guide.pdf)

### Xoserve Supplier API

Bundled by Xoserve as the Supply Point Enquiry service, part of the Gas Enquiry Service (GES). Exposes detailed supply meter point information, filtered by MPRN, address_id or postcode combined with optional address detail. Aimed at organisations working to improve change-of-supply and data assurance processes. Access is arranged through the Retail Energy Code Company (RECCo), not directly with Xoserve.

- **Human URL:** [https://discoveryapiportal.correla.com/api-details#api=supplier&operation=getSupplier_1](https://discoveryapiportal.correla.com/api-details#api=supplier&operation=getSupplier_1)
- **Base URL:** `https://discoveryapi.correla.com/supplier/v1`

#### Tags

- Gas
- Supply Point
- Switching
- Retail Energy Code

#### Properties

- [OpenAPI](openapi/xoserve-supplier-api-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [API Reference](https://discoveryapiportal.correla.com/api-details#api=supplier&operation=getSupplier_1)
- [Documentation](https://www.xoserve.com/products-services/data-products/gas-apis/)
- [Documentation](https://www.xoserve.com/products-services/data-products/gas-enquiry-service-ges/)
- [Portal](https://discoveryapiportal.correla.com/)
- [Sign Up](https://discoveryapiportal.correla.com/signin)

### Xoserve Meter Asset API v1

Bundled by Xoserve as the Meter Asset Enquiry service. Exposes detailed meter asset information for GB gas meter points, filtered by MPRN and/or MSN (meter serial number). Intended for organisations improving data assurance processes. Access is arranged through the Retail Energy Code Company (RECCo).

- **Human URL:** [https://discoveryapiportal.correla.com/api-details#api=meter-asset&operation=getMeterAsset_1](https://discoveryapiportal.correla.com/api-details#api=meter-asset&operation=getMeterAsset_1)
- **Base URL:** `https://discoveryapi.correla.com/meter-asset/v1`

#### Tags

- Gas
- Meter Asset
- Data Assurance

#### Properties

- [OpenAPI](openapi/xoserve-meter-asset-api-v1-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [API Reference](https://discoveryapiportal.correla.com/api-details#api=meter-asset&operation=getMeterAsset_1)
- [Documentation](https://www.xoserve.com/products-services/data-products/gas-apis/)
- [Portal](https://discoveryapiportal.correla.com/)

### Xoserve Meter Asset API v2

Version 2 of the Meter Asset Enquiry API, described by Xoserve as containing additional output fields compared with v1. Same MPRN and MSN filters, same segment-versioned gateway path. This is the version bundled into the current Meter Asset Enquiry product.

- **Human URL:** [https://discoveryapiportal.correla.com/api-details#api=meter-asset-v2&operation=getMeterAsset_1](https://discoveryapiportal.correla.com/api-details#api=meter-asset-v2&operation=getMeterAsset_1)
- **Base URL:** `https://discoveryapi.correla.com/meter-asset/v2`

#### Tags

- Gas
- Meter Asset
- Data Assurance

#### Properties

- [OpenAPI](openapi/xoserve-meter-asset-api-v2-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [API Reference](https://discoveryapiportal.correla.com/api-details#api=meter-asset-v2&operation=getMeterAsset_1)
- [Documentation](https://www.xoserve.com/products-services/data-products/gas-apis/)
- [Portal](https://discoveryapiportal.correla.com/)

## Common Properties

- [Website](https://www.xoserve.com/)
- [Portal](https://discoveryapiportal.correla.com/)
- [Documentation](https://www.xoserve.com/products-services/data-products/gas-apis/)
- [Sign Up](https://discoveryapiportal.correla.com/signin)
- [Pricing](https://www.xoserve.com/media/7975/xoserve-try-before-you-buy-api-service-subscription-guide.pdf)
- [Blog](https://www.xoserve.com/news/)
- [LinkedIn](https://www.linkedin.com/company/xoserve/)
- [Support](https://www.xoserve.com/help-and-support/)
- [Terms of Service](https://www.xoserve.com/terms-and-conditions/)
- [Privacy Policy](https://www.xoserve.com/privacy-notice/)

## Mandate and Access Posture

- **Mandate regime:** Other — Uniform Network Code (UNC) and Data Services Contract (DSC), plus Retail Energy Code (REC) obligations for the Gas Enquiry Service APIs. This is a market-infrastructure mandate on a monopoly central data provider, not a consumer data right. Not the Australian CDR, not Green Button.
- **Mandate status:** Live and implemented — verified by an anonymously readable API catalogue, four provider-generated OpenAPI exports, and a live gateway returning HTTP 401 (not 404) on every documented route.
- **Data standard:** No standard reference found. Proprietary GB gas market schema (MPRN, MSN, AQ, SOQ, exit zone, LDZ). ENTSO-E Energy Identification Codes and Market Domain Data are published as documents, not APIs.
- **Consumer data API:** Yes, but by industry accreditation rather than consumer consent — a licensed Shipper or REC party can retrieve an individual meter point's quantities and supplier detail; a consumer or their agent cannot.
- **Open market data:** No. Xoserve publishes no keyless machine-readable market or system data API. Its public outputs are the Market Domain Data spreadsheet and the free findmysupplier consumer lookup it operates.
- **Access gate:** Accredited only. Browsing the portal is open; keys require eligible licensed GB gas industry status plus a countersigned contract, arranged by email with Xoserve's Customer Life Cycle team or with RECCo for the GES APIs.
- **Auth model:** API key — `APIKey` header or `subscription-key` query parameter (Azure API Management). No OAuth 2.0, no OpenID Connect, no mTLS.
- **Home market:** United Kingdom (Great Britain gas market).

See [review.yml](review.yml) for the full probe log, evidence, and provenance of every harvested artifact.
