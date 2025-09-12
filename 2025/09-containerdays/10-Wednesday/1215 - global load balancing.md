# Evaluating Global Load Balancing Options for Kubernetes in Practice (Nicolai Ort, Tobias Schneck)

2 ways of building multi-cluster redundancy: 

- clustermesh with e.g. cilium, so services can span multiple clusters transparently

- gloabl DNS load balancing, where you point your DNS zone to your coreDNS pod
with a relatively short TTL. if your cluster is down or there are no available
endpoints, no traffic will be directed

words of warning: ideally pay a cloud provider to do that kind of complicated
stuff. or even better use anycast :)


