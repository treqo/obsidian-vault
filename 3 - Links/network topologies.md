```ad-summary
A **network topology** is the physical and logical representation of a network, as nodes (computers) and edges (direct or indirect). In many ways, a network topology is just an application of [[graph theory]] to represent a network.

- "devices" entails, *switches*, *routers*, and software with switch / route functionality.

![[Pasted image 20240725095812.png]]
```

### Mesh Network

```ad-summary
- Every device is connected to every other device (i.e. *complete graph*), leading to a decentralized, interconnected network.
- Highly reliable and robust: if one node fails, the network is essentially unaffected. The other devices can communicate freely
- No hiearchy of power. Every device (node) is equal.
- Expensive and complex to install due to the high number of connections

Example: *P2P networks*
![[Pasted image 20240725095557.png]]

```

### Bus Network

```ad-summary
Every node is connected in series across a single cable. Typically found in cable broadband distribution networks.
- Easily scalable. Just add another node to the connection, or remove a node from the connection without much hassle.
- Not resilient. If something is wrong with the series then all the nodes connected will be affected.

![[Pasted image 20240725100054.png]]
```

### Star Network

```ad-summary
In a star network topology, all the nodes are connected to a single source node, the central hub. A *simple* server-client network with only one server, switched local area networks based on ethernet have a star topology.

![[Pasted image 20240725100537.png]]
```

