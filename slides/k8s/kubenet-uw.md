# Kubernetes network model

- TL,DR:

  *Our cluster (nodes and pods) is one big flat IP network.*

--

- In detail:

 - all nodes must be able to reach each other, without NAT

 - all pods must be able to reach each other, without NAT

 - pods and nodes must be able to reach each other, without NAT

 - each pod is aware of its IP address (no NAT)

- Kubernetes doesn't mandate any particular implementation

---

## Kubernetes network model: the good

- Everything can reach everything

- No address translation

- No port translation

- No new protocol

- Pods cannot move from a node to another and keep their IP address

- IP addresses don't have to be "portable" from a node to another

  (We can use e.g. a subnet per node and use a simple routed topology)

- The specification is simple enough to allow many various implementations

---

## Kubernetes network model: the less good

- Everything can reach everything

  - if you want security, you need to add network policies

  - the network implementation that you use needs to support them

- There are literally dozens of implementations out there

  (15 are listed in the Kubernetes documentation)

- Pods have level 3 (IP) connectivity, but *services* are level 4

  (Services map to a single UDP or TCP port; no port ranges or arbitrary IP packets)

- `kube-proxy` is on the data path when connecting to a pod or container,
  <br/>and it's not particularly fast (relies on userland proxying or iptables)
