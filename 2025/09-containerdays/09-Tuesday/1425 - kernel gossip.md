# Kernel Gossip: What Linux and K8s Are Saying Behind Your Backplane (Nic Vermandé)

## eavesdropping pod creation

creating containers amounts go different syscalls:

1. create a series of namespaces (mount, network, PID, etc.) -> fast (~50 µs)
2. identity & privilege: sethostname(), write to /proc, setresuid. -> fast as well
3. filesystem & rootfs: slow
4.
5.

```yaml
apijversion: kernel.gossip.io/v1alpha
kind: PodBirthCertificate
spec:
  kernel_stats:
    "...": ""
    total_duration_ms: 17
```

## CPU throttling

cAdvisor is a polling-based solution. Periodically reads from the groups
counters, and exposed through kubelet which is then scraped by prometheus

## bpftrace

```
tracepoint:sched:sched_throttle_cfs_rq // immediate results of CFS in a map
```

## kernel-gossip architecture

kernel-observer daemonset: contains an embedded bpftrace loader
kernel-gossip operator: observes ->

- PodBirthCertificate
- KernelWhispers

## Pressure Stall Indicator

<https://docs.kernel.org/accounting/psi.html>
<https://kubernetes.io/docs/reference/instrumentation/understand-psi-metrics/>

Kernel-Native pressure gauge

beta with k8s v1.34: PSI measures wasted time. metrics available in
`/proc/pressure` as cumulative counters
