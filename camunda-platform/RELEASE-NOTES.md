The changelog is automatically generated using [git-chglog](https://github.com/git-chglog/git-chglog)
and it follows [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) format.


<a name="camunda-platform-8.3.5"></a>
## [camunda-platform-8.3.5](https://github.com/camunda/camunda-platform-helm/compare/camunda-platform-8.3.4...camunda-platform-8.3.5) (2024-01-08)

### Verification

To verify integrity of the Helm chart using [Cosign](https://docs.sigstore.dev/signing/quickstart/):

```shell
cosign verify-blob camunda-platform-8.3.5.tgz \
  --bundle camunda-platform-8.3.5.cosign.bundle \
  --certificate-oidc-issuer "https://token.actions.githubusercontent.com" \
  --certificate-identity "https://github.com/camunda/camunda-platform-helm/.github/workflows/chart-release.yaml@refs/pull/1202/merge"
```
