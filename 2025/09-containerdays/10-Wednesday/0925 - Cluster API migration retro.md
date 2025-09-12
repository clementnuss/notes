# ClusterAPI migration retrospective (Joe Salisbury)

built a cli tool to help with the live migration process.

base idea: import secrets from Vault, add new CAPI nodes and gradually replace
the cluster.

technical challenge: had to create a "hacked" vault version to permit exporting
the k8s pki

small "catch": ClusterAPI will evict old etcd members after the first CAPI node
joins successfully
