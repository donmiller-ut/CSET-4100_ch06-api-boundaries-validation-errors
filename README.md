# Chapter 6: API boundaries, validation, and errors

This database-free teaching project is exported from Northstar commit
`b0860c91329394185096a89b805f14423bc72fbc` with reviewed Chapter 6 corrections.
It is not the later cumulative application and is not suitable for internet exposure.

## Open the lecture project

Clone the student repository or extract the supplied ZIP, then use **File > Open
Folder** in VS Code to open the folder containing `pom.xml`, `mvnw`, and `src`.
This is a complete project. You do not need the textbook repository, its history,
or an export script. Use this version instead of the Chapter 4 or 5 project.

Install JDK 25 and use VS Code with Java support. Bruno and a browser support
the request demonstrations and Swagger UI. No database, Docker, credentials,
or separate Maven installation is required.

For slide 14, open
`src/test/java/com/example/northstarmarket/web/ApiExceptionHandlerTests.java`.
The method `unexpectedFailuresReturnSafeCorrelatedProblemDetails` checks the
safe server-error response. Run it from this project root with
`./mvnw -Dtest=ApiExceptionHandlerTests test` (PowerShell:
`.\mvnw.cmd -Dtest=ApiExceptionHandlerTests test`). These failure fixtures run
only in tests, not as endpoints in the packaged application.

## Build and run

Select an installed JDK 25 by setting `JAVA_HOME` to its JDK directory. Confirm
`java -version` and `./mvnw --version` both report Java 25. The wrapper supplies
Maven 3.9.16, not Java. First builds need access to Maven dependency repositories.

```bash
./mvnw clean verify
java -jar target/northstar-market-0.0.1-SNAPSHOT.jar --server.address=127.0.0.1
```

On PowerShell use `.\mvnw.cmd` instead of `./mvnw`. Keep the Java terminal open.
If port 8080 is occupied, stop only your own process or append `--server.port=8090`
and use that port in clients. `Ctrl+C` stops the server; confirm requests no longer
connect. Restart restores Product 42 and discards changes.

- `GET /api/status`: fixed teaching status, not production readiness.
- `GET /actuator/health`: status `UP`; no database dependency.
- `GET /api/products/42`: seeded Product.
- `/v3/api-docs`: generated implementation documentation.
- `/swagger-ui/index.html`: interactive documentation.

## Client and contract checks

In the Bruno desktop app, open `bruno-ch05` or `bruno-ch06` as a collection and
select the `Local` environment. Start the application first, then send the
numbered requests in order. Use separate scratch requests for lecture experiments.

For automated collection runs, use a separately installed Bruno CLI:

```bash
cd bruno-ch05
bru run --env Local --env-var baseUrl=http://127.0.0.1:8080 --bail
cd ../bruno-ch06
bru run --env Local --env-var baseUrl=http://127.0.0.1:8080 --bail
```

Use a fresh process for repeatable list assertions. The first collection has seven
requests; its missing-Product check uses Chapter 6 Problem Details. The second
has eleven requests. Both capture created IDs instead of assuming a fixed number.
Students do not need the instructor's separate HTTP verification tools.

`contract/northstar-product-api.yaml` is this stage's reference. It intentionally
changes Chapter 5 error media/body and create-null-active status; it is not a
claim of backward-compatible error evolution. Authentication expectations are
documented but remain unenforced. Do not use later Category or Order schemas.

## What to inspect

The `product` package separates request DTOs, response DTOs, application values,
mapping, and in-memory state. `web` holds correlation and MVC error translation.
The two custom field constraints preserve Unicode code-point limits and numeric
price increments. The replacement constraint distinguishes omitted description
from explicit null. Framework error headers are retained, including `Allow`.

Tests include the original 18 cases, 12 adapted Chapter 5 contract regressions,
six additional boundary tests, and four nested-validation/server-failure tests.
The appointment fixture is test-only; it adds no production endpoint or patient
data. It demonstrates cascading validation and the consequence of removing it.

The app does not implement persistence, authentication, ownership, rate limiting,
or an operational logging pipeline. Correlation headers alone do not create
searchable diagnostic records. Advice covers exceptions resolved through MVC,
not every container, filter, or transport failure. Jackson conversion is not a
complete JSON Schema validator. Generated schemas still require comparison
against the stage contract, especially custom constraints and nullable fields.
