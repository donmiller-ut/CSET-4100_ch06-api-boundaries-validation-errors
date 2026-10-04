# Chapter 6 Product contract

This stage-specific OpenAPI 3.1.1 contract starts from the Chapter 6 version at
commit `b0860c91329394185096a89b805f14423bc72fbc`. It preserves Product success
shapes without later Category or Order additions and records Problem Details.
Item GET and DELETE explicitly include invalid-identifier `400` responses.

The Chapter 3 baseline remains unchanged. This stage intentionally changes the
error contract from `application/json` and `ApiError` to `application/problem+json`
and Problem Details. `message` becomes `detail`; clients must adapt. Create
`active: null` now fails conversion with `400` rather than Chapter 5's manual `422`.
These teaching-stage changes would require compatibility planning for deployed clients.

Open `northstar-product-api.yaml` in this folder to inspect the reference contract.
Compare it with `/v3/api-docs` from the running application. Build and test the
project using the Maven Wrapper commands in the project README. The instructor's
repository-only contract validator is not required for this standalone project.

The bearer-token expectations are future policy, not implemented Chapter 6 behavior.
Spring/Jackson can coerce some inputs and the DTO rejects blank names more strictly
than this schema's minimum length. Do not claim full JSON Schema input enforcement.
