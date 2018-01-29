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