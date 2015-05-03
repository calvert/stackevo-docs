---
title: Thoughts a New Transport Encapsulation Architecture
abbrev: New TEA
docname: draft-trammell-stackevo-newtea-01
date: 2015-05-02
category: info
ipr: trust200902

author:
 -
  ins: B. Trammell
  name: Brian Trammell
  org: ETH Zurich
  email: ietf@trammell.ch
  street: Gloriastrasse 35
  city: 8092 Zurich
  country: Switzerland

normative:

informative:
  RFC7258:
  RFC7435:
  I-D.ietf-rtcweb-overview:
  I-D.hildebrand-spud-prototype:


--- abstract

This document explores architectural considerations for using encapsulation in support of stack evolution and new transport protocol deployment in an increasingly encrypted Internet. These architectural


It aims eventually to enumerate a set of architectural assumptions for transport evolution based upon new encapsulations, and discuss limitations on the vocabulary used in each of these new interfaces necessary to achieve deployment

--- middle

# Introduction and Background

The current work of the IAB IP Stack Evolution Program is to support the evolution of the Internet's transport layer and its interfaces to other layers in the Internet Protocol stack. The need for this work is driven by two trends. First is the development and increased deployment of cryptography in Internet protocols to protect against pervasive monitoring {{RFC7258}}, which will break many middleboxes used in the operation and management of Internet-connected networks and which assume access to plaintext content. An additional encapsulation layer to allow selective, explicit metadata exchange between the endpoints and devices on path to replace ad-hoc packet inspection is one approach to retain network manageability in an encrypted Internet.

Second is the increased deployment of new applications (e.g. interactive media as in RTCWEB {{I-D.ietf-rtcweb-overview}}) for which the abstractions provided by today's transport APIs (i.e., either a single reliable stream as in SOCK_STREAM over TCP, or an unreliable, unordered packet sequence as in SOCK_DGRAM over UDP) are inadequate. This evolution is constrained by the presence of middleboxes which interfere with connectivity or packet invariability in the presence of new transport protocols or transport protocol extensions.

Parts of this problem are presently being addressed in various ways by the IETF. The Transport Services (TAPS) Working Group is defining a new abstract interface for specifying transport requirements to the transport layer, with a vocabulary based upon existing transport protocol service features. This will allow future transport layers (implemented in userspace libraries, in operating system kernels, or some combination of the two) to select a wire protocol based upon these requirements and the properties of the path between the endpoints, including the impairments of middleboxes along that path.

The Substrate Protocol for User Datagrams (SPUD) Birds of a Feather (BoF) session at IETF 92 in Dallas in March 2015 discussed use cases and a prototype protocol {{I-D.hildebrand-spud-prototype}} for encapsulating opaque content in UDP, with a facility for signaling limited transport semantics and binding metadata to packets and flows in a flexible way. This encapsulation is designed to provide explicit cooperation between endpoints and middleboxes where this makes sense, while allowing new transport protocol development to happen both in the kernel -- to which it has largely been restricted due to the history of the development of TCP/IP -- as well as in userspace. The outcome of the BoF session was to continue the discussion about the architecture, transport semantics and metadata vocabulary, and experimental implementation of this approach on the mailing list established for the BoF (spud@ietf.org)

SPUD is not the only protocol-level work to address explicit communication between endpoints and devices along the path: work in the Transport Layer Security (TLS) working group {{I-D.huitema-tls-dtls-as-subtransport}} discusses the possibility and provides a gap analysis for running a "minimal common subtransport" exposing common transport semantics as in SPUD directly over the Datagram Transport Layer Security (DTLS) protocol {{dtls}}.

These efforts aim at building flexible mechanisms to solve the problem of expanding the interface between the transport layer and the applications above it as well as the problem of making explicit the contract between the transport layer and devices on path which should, in an end-to-end Internet, limit themselves to lower-layer interactions, but practically speaking have not done so for the past two decades.

This document aims to provide an architectural basis for these efforts, enumerating a set of architectural assumptions for transport evolution based upon new encapsulations, and discussing limitations on the vocabulary used in each of these new interfaces necessary to achieve deployment.

# Terminology

This document borrows terminology from {{I-D.ietf-taps-transports}}, specifically Transport Service, Transport Service Feature, Transport Protocol, and Transport Protocol Component, for discussing the composition of transport services.

[EDITOR'S NOTE: define Application Endpoint, Endhost, and Routable Endpoint, as well as Midpoint, Middlebox, etc., using existing terminology where applicable. A defined terminology here will help avoid imprecision in this conversation.]

# An Architecture for Explicit Cooperation with the Path

where we want to be:

STOP MAKING ASSUMPTIONS: make every element of an end-to-end interaction between applications explicit -- either explicitly addressable or at least explicitly discoverable.

- paths treated explicitly - explicitly addressable by endpoint applications, with explicit communication using addressing that makes sense when necessary.

- applications can clearly define transport requirements (instead of implicitly lensing them through known implementations of each socket type). these transport requirements can be exposed to devices along the path where that is useful.

- applications can clearly define trust relationships with each other and with elements along the path (including explicitly untrusted). cite SCION here?

where we think we can get:

we want this to be an evolutionary change, so it has to deploy in the present internet -> UDP-based encapsulations, existing protocols for NAT traversal where necessary (ICE), existing cryptographic protocols (DTLS). no explicit trust with the path yet.

# Unorganized musings from the previous revision of this document

## Implicit trust in endpoint-path signaling

In a perfect world, the trust relationships among endpoints and elements on path would be precisely and explicitly defined: an endpoint would explicitly delegate some processing to a network element on its behalf, network elements would be able to trust any command from any endpoint, and the integrity and authenticity of signaling in both directions would be cryptographically protected.

However, both the economic reality that the users at the endpoints and the operators of the network may not always have aligned interests, as well as the difficulty of universal key exchange and trust distribution among widely heterogeneous devices across administrative domain boundaries, require us to take a different approach toward building deployable, useful metadata signaling.

We observe that imperative signaling approaches in the past have often failed in that they give each actor incentives to lie. Markings to ask the network to explicitly treat some packets as more important than others will see their value inflate away -- i.e., most packets from most sources will be so marked -- unless some mechanism is built to police those markings. Reservation protocols suffer from the same problem: for example, if an endpoint really needs 1Mbps, but there is no penalty for reserving 1.5Mbps "just in case", a conservative strategy on the part of the endpoint leads to over-reservation.

### Declarative marking

An alternate approach is to treat these markings as declarative and advisory, and to treat all markings on packets and flows as relative to other markings on packets and flows from the same sender. In this case, where neither endpoints nor path elements can reliably predict the actions other elements in the network will take with respect to the marking, and where no endpoint can ask for special treatment at the expense of other endpoints, the incentives to marking inflation are greatly diminished.

### Verifiable marking

Second, markings and declarations should be defined in such a way that they are verifiable, and verification built end to endpoints and middleboxes wherever practical. Suppose for example an endpoint declares that it will send constant-bitrate, loss-insensitive traffic at 192kbps. The average data rate for the given flow is trivially verifiable at any endpoint. A firewall which uses this data for traffic classification and differential quality of service may spot-check the data rate for such flows, and penalize those flows and sources which are clearly mismarking their traffic.

### Mark reputation

It is probably not possible, especially in an environment with ubiquitous opportunistic encryption {{RFC7435}}, to define a useful marking vocabulary such that every marking will be so easily verifiable. However, in an environment in which markings are implicitly trusted but verified, the trustworthiness of each endpoint and path can be independently assessed by any node involved in a communication, and reputation-tracking approaches can be used to signal how believable a declaration is to transport protocols which use those declarations, as well as to provide an additional incentive to mark honestly.

Network address translation, of course, makes it difficult to identify nodes to which to assign reputation, in the absence of some cryptographically protected identity. Encapsulation approaches can help make reputation-tracking more feasible by at least making it difficult for an attacker to spoof an endpoint or node in order to ruin its reputation.

The possibility to assign reputation to metadata has interface implications, as well. A transport layer which uses reputation or other trustworthiness information about metadata received from the path should make that reputation information available to the application. Conversely, a transport layer interface that allows an application to expose information about its traffic to the path should be designed to make honest declarations easier to make than dishonest ones, e.g. by defaulting to making declarations based on locally measured quanitites.

## Encapsulations are narrow

A good deal of experience with tunnels has shown that the per-stream overhead of a given encapsulation is generally less important than its impact on MTU. For instance, the SPUD prototype as presently defined needs at least 20 additional bytes in the header per packet: 2 each for source and destination UDP port, 2 for UDP length, 2 for UDP checksum, 8 to identify tubes, 1 for control bits for SPUD itself, and 3 for the smallest possible CBOR map containing a single opaque higher layer datagram. For 1500-byte Ethernet frames, the marginal cost of SPUD before is therefore 1.33% in byte terms, but it does imply that 1450 byte application datagrams will no longer fit in a single SPUD-over-UDP-over-IPv4-over Ethernet packet.

This fact has two implications for encapsulation design: First, maximum payload size per packet should be communicated up to the higher layer, as an explicit feature of the transport layer's interface. Second, link-layer MTU is a fundamental property of each link along a path, so any signaling protocol allowing path elements to communicate to the endpoint should treat MTU as one of the most important properties along the path to explicitly signal.

# IANA Considerations

This document has no considerations for IANA.

# Security Considerations

This revision of this document presents no security considerations. A more rigorous definition of the limits of declarative and verifiable marking would need to be evaluated against a specified threat model, but we leave this to future work.

# Acknowledgments

Many thanks to the attendees of the IAB Workshop on Stack Evolution in a Middlebox Internet (SEMI) in Zurich, 26-27 January 2015; most of the thoughts in this document follow directly from discussions at SEMI. This work is partially supported by the European Commission under Grant Agrement FP7-318627 mPlane; support does not imply endorsement by the Commission of the content of this work.
