# Ticket

<!-- Please note that this repository is for JavaScript / TypeScript related issues only. If you have a question about the SAP Cloud SDK for Java open a question on StackOverflow: https://stackoverflow.com/questions/tagged/sap-cloud-sdk+java -->

## Describe the bug

When using the openapi-generator to generate the SAP DMC API for [Shop Floor Control Production Activities](https://api.sap.com/api/sapdme_sfc/cloud-sdk/JavaScript) it creates warnings like this:

>WARN     (openapi-generator): Could not parse media type, because it is not 'application/json'. Generation will continue with 'any'. This might lead to errors at runtime.

Checking the generated api, some response types are set to `any` even though they are created.

## To Reproduce

Steps to reproduce the behavior:

1. npm init
1. npm install @sap-cloud-sdk/openapi-generator
1. place the API specification of the "Shop Floor Control Production Activities" API in a new folder `resources/service-specs`
1. Replace line 835 `"in": "path",` with `"in": "query",` (this is an other issue)
1. npx openapi-generator --input resources/service-specs --outputDir src/generated

## Expected behavior

The openapi-generator should generate the api without warnings and the generated API shouldn't have "any" as response types.

E.g.:
Open the generated `src/generated/sfc-processing-api.ts` and search for `any`. E.g. for method createSfcsStart:

```ts
createSfcsStart: (
    body: StartSfcRequest,
    queryParameters?: { async?: boolean }
) =>
    // any as the response type
    new OpenApiRequestBuilder<any>('post', '/sfcs/start', {
        body,
        queryParameters
}),
```

it should be

```ts
createSfcsStart: (
    body: StartSfcRequest,
    queryParameters?: { async?: boolean }
) =>
    // StartSfcResponse as the correct response type
    new OpenApiRequestBuilder<StartSfcResponse>('post', '/sfcs/start', {
        body,
        queryParameters
}),
```

## Screenshots

No Screenshots

## Used Versions

- node version via `node -v`: 18.13.0
- npm version via `npm -v`: 9.6.4
- SAP Cloud SDK version you used as dependency:
  - @sap-cloud-sdk/openapi-generator: 3.1.1

## Code Examples

See exampe in this [repo](https://github.com/thakrawielitzki/openapi-generator-bug) and find the downloaded [service definition from 18.04.2023](https://github.com/thakrawielitzki/openapi-generator-bug/tree/main/resources/service-specs-fixed).

## Log file

CLI Output:

```
[2023-04-20T14:29:55.720Z] INFO     (cli): Parsing args...
[2023-04-20T14:29:55.836Z] WARN     (openapi-generator): Could not parse media type, because it is not 'application/json'. Generation will continue with 'any'. This might lead to errors at runtime.
[2023-04-20T14:29:55.837Z] WARN     (openapi-generator): Could not parse media type, because it is not 'application/json'. Generation will continue with 'any'. This might lead to errors at runtime.
[2023-04-20T14:29:55.838Z] WARN     (openapi-generator): Could not parse media type, because it is not 'application/json'. Generation will continue with 'any'. This might lead to errors at runtime.
[2023-04-20T14:29:55.838Z] WARN     (openapi-generator): Could not parse media type, because it is not 'application/json'. Generation will continue with 'any'. This might lead to errors at runtime.
[2023-04-20T14:29:55.839Z] WARN     (openapi-generator): Could not parse media type, because it is not 'application/json'. Generation will continue with 'any'. This might lead to errors at runtime.
[2023-04-20T14:29:55.840Z] WARN     (openapi-generator): Could not parse media type, because it is not 'application/json'. Generation will continue with 'any'. This might lead to errors at runtime.
[2023-04-20T14:29:55.842Z] WARN     (openapi-generator): Could not parse media type, because it is not 'application/json'. Generation will continue with 'any'. This might lead to errors at runtime.
[2023-04-20T14:29:56.235Z] INFO     (openapi-generator): Successfully generated client for 'C:\<path>\openapi-generator-bug\resources\service-specs-fixed\sapdme_sfc.json'
[2023-04-20T14:29:56.236Z] INFO     (openapi-generator): Finished generation. Don't forget to add @sap-cloud-sdk/openapi to your dependencies.
[2023-04-20T14:29:56.236Z] INFO     (cli): Generation of services finished successfully.
```

## Impact / Priority

<!--
 Please briefly state how this issue impacts your project and what your timeline is.
 -->

Affected development phase: **Development**

Impact: e.g. **Inconvenience**

Timeline: e.g. Go-Live is in 12 weeks.

## Additional context

Add any other context about the problem here.
