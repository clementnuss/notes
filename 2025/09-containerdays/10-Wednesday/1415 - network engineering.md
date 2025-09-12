# Kubernetes Traffic Engineering Right on Track (Piotr Jabłoński)

Some naming conventions:

- flow = uni/bi directional set of packets with same attributes
  (5-tuple: source IP, dest IP, source port, dest port, protocol)
- traffic = aggregate of packets/flows across a network or system

## K8s Inbound traffic

external traffic policy (eTP) advantages: no SNAT / keep source IP. however load balancing can be bad

also when having too many nodes if they all advertise services with BGP you can
have issues. solution: -> add BGP-nodes which act as a front-end and route the
traffic to pods. with the you have proper load balancing

## K8s Egress traffic

e.g. cilium egress gateway adds control for traffic routing/filtering.

egw policy adds options for topology aware routing (i.e. routing to AZ east when you're in DC east)
