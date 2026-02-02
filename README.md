# redis-umbrella

Umbrella Helm chart that deploys two Redis instances using the Bitnami Redis chart as aliased dependencies:
- redis-front (alias -> redis chart @ version you choose)
- redis-back  (alias -> redis chart @ a different version)

## Quick start

1. Add the Bitnami repo and update:

   ```bash
   helm repo add bitnami https://charts.bitnami.com/bitnami
   helm repo update
   ```

2. From the chart directory (where Chart.yaml is), update dependencies:

   ```bash
   helm dependency update ./redis-umbrella
   ```

   This will download both redis charts (with alias metadata) into `./redis-umbrella/charts/`.

3. Edit `values.yaml` to:
   - Set secure passwords (or configure to use existing Kubernetes Secrets)
   - Tune resource/persistence settings
   - Change versions in Chart.yaml if needed

4. Install the umbrella chart:

   ```bash
   helm install my-redis ./redis-umbrella -f ./redis-umbrella/values.yaml
   ```

5. Get the Redis passwords and connect (see NOTES.txt or the commands below):

   ```bash
   kubectl get secret --namespace default my-redis-redis-front -o jsonpath="{.data.redis-password}" | base64 --decode
   kubectl get secret --namespace default my-redis-redis-back  -o jsonpath="{.data.redis-password}" | base64 --decode
   ```

## Using pre-created Kubernetes Secrets (recommended for production)

Bitnami Redis chart supports using an existing secret in many versions (check your chart's values schema). Example pattern:

```yaml
redis-front:
  auth:
    enabled: true
    existingSecret: "my-redis-front-secret"
    existingPasswordKey: "redis-password"

redis-back:
  auth:
    enabled: true
    existingSecret: "my-redis-back-secret"
    existingPasswordKey: "redis-password"
```

Create the secrets beforehand:

```bash
kubectl create secret generic my-redis-front-secret --from-literal=redis-password='SOME_SECURE_PASS'
kubectl create secret generic my-redis-back-secret  --from-literal=redis-password='ANOTHER_SECURE_PASS'
```

## Notes & best practices

- Always pin dependency versions in Chart.yaml (as shown) to avoid unexpected breaking changes.
- Validate the exact values keys for the exact Bitnami Redis versions you pick — upstream keys can change between chart versions.
- Don't store plaintext passwords in Git. Use SealedSecrets, HashiCorp Vault, External Secrets, or Helm secrets tooling.
- If you need replication/clustered Redis for either front or back, enable and configure `cluster:`/`replication:` based on the specific chart's supported keys.

## Want help with:
- Creating Kubernetes Secret manifests (or SealedSecrets) and wiring them into this chart?
- A version matrix and recommended Bitnami versions for compatibility?
- A Helmfile / GitHub Actions workflow to package and publish this umbrella chart?

If yes — tell me which and I’ll generate it.