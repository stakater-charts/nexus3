# chart-nexus3
This repository contains 2 charts that are used to deploy nexus3 to kubernetes.
- nexus3-storage
- nexus3

## Installing
First install `nexus3-storage` chart
```
helm install --name nexus3-storage chartmuseum/nexus3-storage
```

After that, install `nexus3` chart
```
helm install --name nexus3 chartmuseum/nexus3
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