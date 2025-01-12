**Convert and list cpu and memory**
```bash
kubectl get nodes -o json | jq -r '.items[] | "\(.metadata.name): CPU = \((.status.capacity.cpu | tonumber) * 1000) m, Memory = \((.status.capacity.memory | sub("Ki$"; "") | tonumber) / 1048576 | round) GB"'
```
**output example:**
```bash
cp-master: CPU = 3000 m, Memory = 4 GB
worker-01: CPU = 2000 m, Memory = 4 GB
```
