# ClusterOne

Blueprint for an [Edka](https://edka.io) Kubernetes cluster. For usage details, see the [docs](https://docs.edka.io/get-started/build-your-own-paas).

## Repository Structure

```

├── clusters/               # Directory for multiple cluster configurations
│   ├── clusterone/         # Configuration manifests for a cluster named `clusterone`
│   │   ├── cluster-secrets-store.yaml
│   │   ├── namespaces.yaml
│   │   ├── postgres.yaml
│   │   └── shared.yaml
│   ├── resources/          # Final manifests referenced from [`clusters/clusterone`](clusters/clusterone)
│   │   └── clusterone/
│   │       ├── postgres/
│   │       └── secrets/
│   └── shared/             # Shared manifests that can be used across multiple clusters
│       └── helmrepository.yaml
```

### Cluster Configuration Manifests [`clusters/clusterone`](clusters/clusterone)
This directory contains the primary manifests for a cluster named `clusterone` in Edka. Any YAML manifest added here will be applied to the cluster. 

**Note:** *Kustomization is used to manage resource dependencies and installation order.*

- **`cluster-secrets-store.yaml`**: Integrates External Secrets to sync secrets from providers like Doppler, avoiding hardcoded secrets in Git. Its resources are located in [`clusters/resources/clusterone/secrets`](clusters/resources/clusterone/secrets/).

- **`namespaces.yaml`**: Defines Kubernetes namespaces for organizing resources and add-ons (e.g., production, preview, etc.).

- **`postgres.yaml`**: Configures a PostgreSQL Cluster using the Cloud Native PostgreSQL operator with HA, backups, and PITR from AWS S3. Its resources are located in [`clusters/resources/clusterone/postgres`](clusters/resources/clusterone/postgres/).

- **`shared.yaml`**: Sets up shared configurations that usually are the same for all your clusters. It references shared manifests in [`clusters/shared`](clusters/shared).

### Resources [`clusters/resources`](clusters/resources)
This directory holds the detailed resource configurations, organized by cluster name. These are referenced by the manifests within each cluster's specific directory (e.g., `clusters/clusterone`).

### Shared Configurations [`clusters/shared`](clusters/shared)
This directory contains shared configurations that can be used across multiple clusters. For example, the `helmrepository.yaml` file defines a Helm repository to allow pulling private Helm charts from a private repository.
