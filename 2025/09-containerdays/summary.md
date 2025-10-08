# ContainerDays 2025 Summary

## Tuesday Highlights

### Attacker Persistence Strategies in Kubernetes (Rory McCune) - 09:20

- Scenario: stolen cluster-admin credentials in Hamburg cafe
- Attack techniques:
  - Use `ctr` to start hidden containers
  - Create static pod manifests with invalid namespaces
  - Establish tunnels (Tailscale) for persistent access
- Privilege escalation via kubelet API and CSR/TokenRequest APIs
- `clusterrole-aggregation-controller` service account can escalate permissions
- Defense: audit logs, node agents, least privilege RBAC

### Dynamic OCI Registry (Alvaro Hernandez) - 10:05

- DOCIR: solves the combinatorial explosion problem of container variants
- Dynamic image composition for PostgreSQL extensions (~1,000 available)
- URL-based extension encoding: `postgres--16.3/e/ext1--v1/ext2--v2`
- Avoids "fatty containers" and pre-generating all combinations
- Built with Java 21 + Quarkus

### Dynamic Multi-Cluster Controllers (Marvin Beckers & Stefan Schimanski) - 10:55

- Multicluster-runtime: friendly extension of controller-runtime
- Scaling models: process manager vs in-process manager
- Pluggable providers (kubeconfig, kind, cluster-inventory-api)
- Uniform and multi-cluster-aware reconcilers
- Stable release coming soon with sharded controller support

### Container Philosophy (Kelsey Hightower) - 13:30

- "Why are we still talking about containers?" - because there's still work to do
- Products ship fast but are never complete

### eBPF Made Easy (Qasim Sarfraz, Michael Friese) - 16:45

- Inspektor Gadget: packages eBPF programs into OCI images called "Gadgets"
- Orchestrates eBPF programs on nodes
- Exports metrics to OpenTelemetry enriched with K8s metadata

## Wednesday Highlights

### Introducing KRO (Abdel Sghiouar) - 10:10

- Kubernetes Resource Orchestrator: "solution to the terraform problem"
- Simplifies CRD and operator creation
- `ResourceGraphDefinition` declaratively defines new APIs
- Template-based resource management with variable substitution
- Eliminates need for complex operator development

### Container Runtime Deep Dive (Ajitem Sahasrabuddhe) - 11:30

- Hands-on session building a container runtime from scratch in Go
- Demo code available at github.com/asahasrabuddhe/gcr

