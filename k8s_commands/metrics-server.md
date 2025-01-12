**Convert and list full cpu and memory:**
```bash
kubectl get nodes -o json | jq -r '.items[] | "\(.metadata.name): CPU = \((.status.capacity.cpu | tonumber) * 1000) m, Memory = \((.status.capacity.memory | sub("Ki$"; "") | tonumber) / 1048576 | round) GB"'
```
**Example output:**
```bash
cp-master: CPU = 3000 m, Memory = 4 GB
worker-01: CPU = 2000 m, Memory = 4 GB
```

**Convert and list usage cpu and memory:**
```bash
kubectl get --raw /apis/metrics.k8s.io/v1beta1/nodes | jq -r '.items[] | .metadata.name + ": CPU = " + ((.usage.cpu | sub("n$"; "") | tonumber) / 1000000 | (. * 100 | round) / 100 | tostring) + " m, Memory = " + ((.usage.memory | sub("Ki$"; "") | tonumber) / 1048576 | (. * 100 | round) / 100 | tostring) + " GB"'
```
**Example output:**
```bash
cp-master: CPU = 121.51 m, Memory = 2.06 GB
worker-01: CPU = 41.86 m, Memory = 1.24 GB
```
