# Kong JWT Claims to Headers Plugin (`kong-plugin-jwt-claims-headers`)

> Modified Kong JWT plugin to inject all serializable JWT claims as `X-Jwt-Claim-*` upstream headers, including JSON array formatting.

This plugin enhances the standard Kong JWT authentication by automatically adding claims found within a validated JWT to the headers of the request sent to the upstream service. This simplifies accessing user information (like User ID from `sub`) and authorization details (like `roles`) in downstream services without requiring them to parse the JWT again.

## Features

*   Authenticates requests using JWTs (based on official Kong JWT plugin logic).
*   Injects **all** serializable JWT claims into upstream request headers.
*   Headers are prefixed with `X-Jwt-Claim-` (e.g., `X-Jwt-Claim-sub`, `X-Jwt-Claim-roles`).
*   Simple claim types (string, number, boolean) are passed as their string representation.
*   Array claims containing simple types are serialized as **JSON array strings** (e.g., `["role1", "role2"]`).
*   No external Lua library dependencies (like `lua-cjson`).

## Configuration

This plugin uses the same configuration parameters as the [official Kong JWT plugin](https://docs.konghq.com/hub/kong-inc/jwt/). It is a modified version from their [source](https://github.com/Kong/kong/tree/master/kong/plugins/jwt). Apply it to a Service, Route, or globally as needed.

## Limitations

*   Only serializes flat arrays of simple types (string, number, boolean) to JSON. Nested objects or arrays with complex types inside the JWT claims will not be added as headers.
*   Relies on the core logic of the official Kong JWT plugin for validation and credential fetching.
