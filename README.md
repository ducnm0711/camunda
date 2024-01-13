```
helm -n infra install camunda-8 camunda-platform -f values.yaml
helm -n infra diff upgrade --install camunda-8 camunda-platform -f values.yaml -f values-elasticsearch.yaml
```

- Multi Region: 1 
- Multi Tenancy: Single
- Global Config - Shared across components
  - Global Identity will override Identity Config - External Keycloak vs Camunda chart Keycloak 

# Disabled components:
- Tasklist
- Operate
- Optimize

# Optional Dependency