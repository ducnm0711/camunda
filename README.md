# Camumda 8

Camunda is a Business Project Management (BPMN) Tool
## Architecture
Camunda Platform 8
- 2 main components
  - Gateway: Client libaray wwill connect to here
  - Zeebe

- Other components (Operate, Optimize, Connectors,...)
## Installation
```
helm -n camunda install camunda-8 camunda-platform -f values.yaml
helm -n camunda diff upgrade --install camunda-8 camunda-platform -f values.yaml -f values-elasticsearch.yaml
```

- Multi Region: 1 
- Multi Tenancy: Single
- Global Config - Shared across components
  - 
  - Global Identity will override Identity Config - External Keycloak vs Camunda chart Keycloak 

# Getting Started
https://docs.camunda.io/docs/self-managed/identity/getting-started/install-identity/

# Disabled components:
- Tasklist
- Operate
- Optimize

# Optional Dependency
## Exporter 
- https://docs.camunda.io/docs/self-managed/concepts/exporters/
> Note: The main impact exporters have on a Zeebe cluster is that they remove the burden of persisting data indefinitely.

Supported exporter type:
- ElasticSearch: https://docs.camunda.io/docs/self-managed/zeebe-deployment/exporters/elasticsearch-exporter/
- OpenSearch: https://docs.camunda.io/docs/self-managed/zeebe-deployment/exporters/opensearch-exporter/
```yaml
global:
  elasticsearch:
    disableExporter: true
```

## Install/Upgrade with Identity Basic auth
```
helm -n camunda diff upgrade --install camunda camunda-platform -f values-no-auth.yaml
```
## Install/Upgrade with Identity Oauth
```
export TASKLIST_SECRET=$(kubectl -n camunda get secret "camunda-8-tasklist-identity-secret" -o jsonpath="{.data.tasklist-secret}" | base64 --decode)
export OPTIMIZE_SECRET=$(kubectl -n camunda get secret "camunda-8-optimize-identity-secret" -o jsonpath="{.data.optimize-secret}" | base64 --decode)
export OPERATE_SECRET=$(kubectl -n camunda get secret "camunda-8-operate-identity-secret" -o jsonpath="{.data.operate-secret}" | base64 --decode)
export CONNECTORS_SECRET=$(kubectl -n camunda get secret "camunda-8-connectors-identity-secret" -o jsonpath="{.data.connectors-secret}" | base64 --decode)
export ZEEBE_SECRET=$(kubectl -n camunda get secret "camunda-8-zeebe-identity-secret" -o jsonpath="{.data.zeebe-secret}" | base64 --decode)
export KEYCLOAK_ADMIN_SECRET=$(kubectl -n camunda get secret "camunda-8-keycloak" -o jsonpath="{.data.admin-password}" | base64 --decode)
export KEYCLOAK_MANAGEMENT_SECRET=$(kubectl -n camunda get secret "camunda-8-keycloak" -o jsonpath="{.data.management-password}" | base64 --decode)
export POSTGRESQL_SECRET=$(kubectl -n camunda get secret "camunda-8-postgresql" -o jsonpath="{.data.postgres-password}" | base64 --decode)
export CONSOLE_SECRET=$(kubectl -n camunda get secret camunda-8-console-identity-secret -o jsonpath="{.data.console-secret}" | base64 -d)
```

```
helm -n camunda diff upgrade --install camunda-8 camunda-platform -f values.yaml\
  --set global.identity.auth.tasklist.existingSecret=$TASKLIST_SECRET \
  --set global.identity.auth.optimize.existingSecret=$OPTIMIZE_SECRET \
  --set global.identity.auth.operate.existingSecret=$OPERATE_SECRET \
  --set global.identity.auth.connectors.existingSecret=$CONNECTORS_SECRET \
  --set global.identity.auth.zeebe.existingSecret=$ZEEBE_SECRET \
  --set global.identity.auth.console.existingSecret=$CONSOLE_SECRET=$ \
  --set identity.keycloak.auth.adminPassword=$KEYCLOAK_ADMIN_SECRET \
  --set identity.keycloak.auth.managementPassword=$KEYCLOAK_MANAGEMENT_SECRET \
  --set identity.keycloak.postgresql.auth.password=$POSTGRESQL_SECRET
```