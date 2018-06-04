# chart-nexus3
This repository contains 2 charts that are used to deploy nexus to kubernetes.
- nexus-storage
- nexus

## Installing
First install `nexus-storage` chart
```
helm install --name nexus-storage chartmuseum/nexus-storage
```

After that, install `nexus` chart
```
helm install --name nexus chartmuseum/nexus
```

### Notes
For secrets, you need to specify them in base64 format of the following files:

- admin_account.json:

```json
{
  "name": "stackator-admin",
  "type": "groovy",
  "content": "security.addUser('stackator-admin', 'Stackator', 'Admin', 'jane.doe@example.com', true, 'REPLACE_THIS_PASSWORD', ['nx-admin'])"
}
```

- cluster_account.json

```json
{
  "name": "stackator-cluster",
  "type": "groovy",
  "content": "security.addRole('cluster', 'cluster', 'User with privileges to allow read access to repo content and healtcheck', ['nx-healthcheck-read','nx-repository-view-docker-stackator-docker-browse','nx-repository-view-docker-stackator-docker-read','nx-search-read'],  ['nx-anonymous']); security.addUser('stackator-cluster', 'Cluster', 'Cluster', 'stakater@gmail.com', true, 'REPLACE_THIS_PASSWORD', ['cluster'])"
}
```
