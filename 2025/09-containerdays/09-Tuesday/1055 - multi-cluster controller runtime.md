# Dynamic Multi-Cluster Controllers with controller-runtime (Marbin Beckers & Stefan Schimanski)

scaling models:

- process manager which creates one controller pod per cluster
- in-process manager manager: create on cluster manager in code/runtime for new clusters

## introducing multicluster-runtime

friendly extension of controller-runtime, built on top of controller-runtime
<https://github.com/kubernetes-sigs/multicluster-runtime>

pluggable provider

### types of reconcilers

- uniform reconcilers, same logic/code for all clusters
- multi-cluster-aware reconcilers

### example of a multicluster-runtime

```golang
// manager setup

provider := kind.New()

mgr, err := mcmanager.New(ctrl.GetConfigOrDie(), provider, optis)
mgr, err := mcmanager.New(ctrl.GetConfigOrDie(), nil, optis) // single-cluster version

// reconciler setup

mcreconcile.Func(
  func(ctx context.Context, req  mcreconcile.Request) (ctrl.Result, error) {
    cl, err := mgr.Getjclusterf9ctx, req.jclusterfname)

    }
  )

```

implemented on top of controller-runtime.

```golang

package multi-cluster-controller

type Request struct {
  reconcile.Request // implements controller-runtime struct
  ClusterName string // empty by default
}
```

<https://github.com/kubernetes-sigs/multicluster-runtime/blob/main/pkg/manager/manager.go>

### providers

- in-repository providers: single, nop, kubeconfig, kind, etc
- recent pull request for cluster inventory provider

<https://github.com/kubernetes-sigs/cluster-inventory-api>

### upcomfing

stable release soon.

sharded controllers support
