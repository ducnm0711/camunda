```
helm install camunda-8 camunda-platform -f values.yaml
```

- Multi Region: 1 
- Multi Tenancy: Single
- Global Config - Shared across components
  - Global Identity will override Identity Config - External Keycloak vs Camunda chart Keycloak 
