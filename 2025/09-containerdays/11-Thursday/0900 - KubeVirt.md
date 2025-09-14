# KubeVirt on the Loose: Kubernetes-Powered VM Migrations That Defy Gravity (Ronny Isaac)

discussion on kube virt live migration capabilities.

key elements are:

- Read-write- many is optional but vastly reduces the live mgiration time.
- dirty memory pages are copied back to the target kube-virt
- memory convergence can only happen if memory change rate is smaller than the maximum network capacity/limit for the memory transfer.
