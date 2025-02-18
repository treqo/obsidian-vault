---
tags:
  - computers
  - computer-engineering
  - "#coding"
  - "#hacking"
time: 2024-07-25T09:34:00
status: Incomplete
---
**Type:** [[website]]  
**Topics:** #  
**Date:** 2024-07-25  
**Source/Author:** {{Source/Author}} 
- **Tags**
	- [[network]] [[network communication]] [[network protocols]] [[network topologies]] [[network mediums]]

---
# Introduction to Networking

## Networking Overview

```ad-quote
title: Big Idea
A [[network]] enables two computers to [[network communication|communicate]] with each other. There is a wide array of [[network topologies|topologies]] (mesh/tree/star), [[network mediums|mediums]] (ethernet/fiber/coax/wireless), and [[network protocols|protocols]] (TCP, UDP, IPX) that can be used to *facilitate the network*.
```

```ad-note
title: Motivation for complex networks
Setting up a large flat network (i.e. on central hub) is easy to do, but poses a security risk: that risk being, one attack can gain access to everything.

By creating lots of smaller networks and interconnecting them, we are able to add more defense layers, divide up secure information. Pivoting around a network is not difficult but doing it *quietly* and *fast* becomes much harder.
```

```ad-summary
title: Defense Techniques
We will create this analogy that your network is a house, and network defense techniques can be thought of as various attributes of the house:
- Building *smaller networks* and putting **Access Control Lists** on them is like building a fence around a house. An attacker could jump over the fence, but this would be very suspicious and can be suspected as malicious activity. Why is the printer network talking to the servers over HTTP? (someone has gained access to the printer network)
- Taking the time to map out each network and its purpose is like placing lights around your house, making sure you can see all activity. If one network is not doing its intended purpose, it may be compromised. Why is the printer network talking to the internet at all?
- Having bushes around windows is a deterrent to people trying to enter through a window. This is analogous to *Intrusion Detection Systems* (Suricata, Snort) a deterrent to running network scans. Why did a port scan originate from the printer network?

*use excalibur to draw an image*
```

![[Pasted image 20240725102403.png]]
*high-level diagram of a Work From Home setup*

- URL (Uniform Resource Locator) – Specifies an exact location in a specific domain
	ex: https://www.hackthebox.eu/example?floor=2&office=dev&employee=17
- FQDN (Fully Qualified Domain Name) – specifies a domain, however there can be different parts to this domain
	ex: www.hackthebox.eu








---

## Questions
- Question 1
- Question 2

## Further Exploration
- Links to related articles, videos, or books

## Personal Insights
- Personal reflections or insights gained from the media


# References




