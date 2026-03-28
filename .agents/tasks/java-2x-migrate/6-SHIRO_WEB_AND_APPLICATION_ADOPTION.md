# Task 6: Shiro, Web Layer, And Application Adoption

## Objective

Adopt the lower-level migration in `killbill` itself, especially in the web
layer and Shiro-driven request flow.

## Primary repo scope

- `killbill`

## Verified anchors

- `killbill/pom.xml`
- `killbill/jaxrs/pom.xml`
- `killbill/profiles/killbill/src/main/webapp/WEB-INF/web.xml`
- `killbill/profiles/killpay/src/main/webapp/WEB-INF/web.xml`
- Shiro-bearing module POMs in `killbill/util`, `killbill/tenant`,
  `killbill/payment`, and `killbill/entitlement`

## Library focus

- `org.apache.shiro:*`
- web descriptor and filter-chain alignment
- JAX-RS adoption of the updated foundation
- remaining legacy servlet and `javax.ws.rs` usage in the app layer

## Deliverables

1. Update application-layer servlet and filter-chain integration to match the
   migrated foundation.
2. Review Shiro web and security coupling under the new stack.
3. Validate `killbill` module and integration tests after adoption.

## Safe point

Application adoption should be isolated from lower-level library migration.
This task should consume upstream changes, not redefine commons or platform
contracts.

## Exit criteria

- `killbill` builds against the updated lower layers.
- Web descriptors and runtime filters are aligned.
- Shiro-related regressions are either fixed or explicitly documented.
