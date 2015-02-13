---
title: IAB Workshop on Stack Evolution in a Middlebox Internet (SEMI) Report
abbrev: SEMI Workshop
docname: draft-trammell-semi-report-00
date: 2015-2-20
category: info
ipr: trust200902


author:
  -
    ins: B. Trammell
    name: Brian Trammell
    role: editor
    organization: ETH Zurich
    email: ietf@trammell.ch
  -
    ins: M. Kuehlewind
    name: Mirja Kuehlewind
    role: editor
    organization: ETH Zurich
    email: mirja.kuehlewind@tik.ee.ethz.ch

informative:
  RFC3234:
  RFC6347:
  draft-hardie-spud-use-cases:
  draft-hildebrand-spud-prototype:

--- abstract

The Internet Architecture Board through its IP Stack Evolution program, the Internet Society, and the Swiss Federal Institute of Technology (ETH) Zurich hosted the Stack Evolution in a Middlebox Internet (SEMI) workshop in Zurich on 26-27 January 2015 to explore the ability to evolve the transport layer in the presence of middlebox- and interface-related ossification of the stack. The goal of the workshop was to produce architectural and engineering guidance on future work to break the logjam, focusing on incrementally deployable approaches with clear incentives to deployment both on the endpoints (in new transport layers and applications) as well as on middleboxes (run by network operators). This document summarizes the contributions to the workshop, provides an overview of the discussion at the workshop, as well as the outcomes and next steps identified

--- middle

# Introduction

The transport layer of the Internet has becomed ossified, squeezed between narrow interfaces (from BSD sockets to pseudo-transport over HTTPS) and increasing in-network modification of traffic by middleboxes that make assumptions about the protocols running through them. This ossification makes it difficult to innovate in the transport layer, through the deployment of new protocols or the extension of existing ones. At the same time, emerging applications require functionality that existing protocols can provide only inefficiently, if at all.

To begin to address this problem, the IAB, within the scope of its IP Stack Evolution Program, organized a workshop to discuss approaches to de-ossifying transport, especially with respect to interactions with middleboxes and new methods for implementing transport protocols. Recognizing that the end-to-end principle has long been compromised, we start with the fundamental question of matching paths through the Internet with certain characteristics to application and transport requirements. 

We posed the following questions in the call for papers: Which paths through the Internet are actually available to applications? Which transports can be used over these paths? How can applications cooperate with network elements to improve path establishment and discovery? Can common transport functionality and standardization help application developers to implement and deploy such approaches in today’s Internet? Could cooperative approaches give us a way to rebalance the Internet back toward its end-to-end roots?

Topics for contributions in the call for papers were identified as follows:

- Development and deployment of transport-like features in application-layer protocols
- Methods for discovery of path characteristics and protocol availability along a path
- Methods for middlebox detection and characterization of middlebox behavior and functionality
- Methods for NAT and middlebox traversal in the establishment of end-to-end paths
- Mechanisms for cooperative path-endpoint signaling, and lessons learned from existing approaches
- Economic considerations and incentives for cooperation in middlebox deployment
- We will explicitly focus on approaches that are incrementally deployable within the present Internet.

This workshop report summarizes the contributions to and discussions at the workshop, organized by topic. We starting with a summary of the current state of the Internet, and explore the incentives which have made it that way and the role of incentives in evolution. Many contributions were broadly split into two areas: middlebox measurement, classification, and approaches to defense against middlebox modification of packets; and approaches to support transport evolution. All accepted position papers are available at https://www.iab.org/activities/workshops/semi/.

The outcomes of the workshop are discussed in {{outcomes}}, and discuss progress after the workshop toward each of the identified work items as of the time of publication of this report.

# Current state of the Internet

[EDITOR'S NOTE: This section is incomplete; see the workshop page at https://www.iab.org/activities/workshops/semi/ for papers and transcripts in the meantime.]

# Incentives for Stack Ossification and Evolution

[EDITOR'S NOTE: This section is incomplete; see the workshop page at https://www.iab.org/activities/workshops/semi/ for papers and transcripts in the meantime.]

# The Role and Rule of Middleboxes

[EDITOR'S NOTE: This section is incomplete; see the workshop page at https://www.iab.org/activities/workshops/semi/ for papers and transcripts in the meantime.]

# Evolving the Transport Layer

[EDITOR'S NOTE: This section is incomplete; see the workshop page at https://www.iab.org/activities/workshops/semi/ for papers and transcripts in the meantime.]

# Outcomes

[EDITOR'S NOTE: frontmatter for outcomes. note that fondue was served.]

## Minimal signaling for encapsulated transports

Assuming that a way forward for transport evolution in user space would be encapsulation in UDP datagrams, the workshop identified the need for a facility built atop UDP to provide minimal signaling of the semantics of a flow that would otherwise be available in TCP: at the very least, indications of first and last packets in a flow to assist firewalls and NATs in policy decision and state maintenance. This facility could also provide minimal application-to-path and path-to-application signaling, though there was less agreement exactly what should or could be signaled here.

It was also noted that, given the increasing deployment of encryption in the Internet, this facility should cooperate with DTLS {{RFC6347}} in order to selectively expose information about traffic flows where the transport headers and payload themselves are encrypted.

To develop this concept further, it was decided to hold a non working group forming BoF session, SPUD (Substrate Protocol for User Datagrams), at the IETF 92 meeting in March in Dallas. A draft on use cases {{draft-hardie-spud-use-cases}} and a prototype protocol specification {{draft-hildebrand-spud-prototype}} were prepared following discussions at SEMI.

## Middlebox measurement

[EDITOR'S NOTE: middlebox measurement bar BoF]

## Guidelines for middlebox deployment

[EDITOR'S NOTE: potential IAB document following from {{RFC3234}}]

## Additional Activities in the IETF

[EDITOR'S NOTE: tsvarea presentation on transport protocol extensibility, tsvarea presentation on general UDP encapsulation considerations, appsarea presentation on the use of TLS/DTLS as middlebox modification and inspection prevention approach. ]

## Additional Activities in Other Venues 

[EDITOR'S NOTE: informal liaison to ETSI NFV ]

# Security Considerations

This document presents no security considerations.

# Acknowledgments

The IAB thanks the SEMI Program Committee: Brian Trammell, Mirja Kuehlewind, Joe Hildebrand, Eliot Lear, Mat Ford, Gorry Fairhurst, and Martin Stiemerling. We additionally thank Prof. Dr. Bernhard Plattner of the Communication Systems Group at ETH for hosting the workshop, and the Internet Society for its support.

# Attendees

The following people attended the SEMI workshop:

Aaron Yi Ding, Aaron Falk, Barry Leiba, Benoit Donnet, Bob Briscoe, Brandon Williams, Ken Calvert, Costin Raiciu, Colin Perkins, David Black, Dave Thaler, Dan Wing, Felipe Huici, Mat Ford, Gorry Fairhurst, Russ Housely, Christian Huitema, Brian Trammell, Joe Hildebrand, Jana Ivengar, Lars Eggert, Eliot Lear, Marc Blanchet, Mary Barns, Michael Welzl, Mirja Kuehlewind, Martin Stiemerling, Miroslav Ponec, Erik Nordmark, Philipp Schmidt, Bernhard Plattner, Richard Barnes, Spencer Dawkins, Szilveszter Nadas, Ted Hardie, Dan Wing, and Xing Li. Additionally, Eric Rescorla and Stuart Cheshire contributed to the workshop but were unable to attend.
