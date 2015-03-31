---
title: IAB Workshop on Stack Evolution in a Middlebox Internet (SEMI) Report
abbrev: SEMI Workshop
docname: draft-trammell-semi-report-00
date: 2015-4-16
category: info
ipr: trust200902


author:
  -
    ins: B. Trammell
    name: Brian Trammell
    role: editor
    org: ETH Zurich
    email: ietf@trammell.ch
    street: Gloriastrasse 35
    city: 8092 Zurich
    country: Switzerland
  -
    ins: M. Kuehlewind
    name: Mirja Kuehlewind
    role: editor
    organization: ETH Zurich
    email: mirja.kuehlewind@tik.ee.ethz.ch
    street: Gloriastrasse 35
    city: 8092 Zurich
    country: Switzerland

informative:
  RFC3234:
  RFC6347:
  RFC7305:
  I-D.hardie-spud-use-cases:
  I-D.hildebrand-spud-prototype:
  I-D.huitema-tls-dtls-as-subtransport:
  I-D.trammell-stackevo-newtea:

--- abstract

The Internet Architecture Board (IAB) through its IP Stack Evolution program, the Internet Society, and the Swiss Federal Institute of Technology (ETH) Zurich hosted the Stack Evolution in a Middlebox Internet (SEMI) workshop in Zurich on 26-27 January 2015 to explore the ability to evolve the transport layer in the presence of middlebox- and interface-related ossification of the stack. The goal of the workshop was to produce architectural and engineering guidance on future work to break the logjam, focusing on incrementally deployable approaches with clear incentives to deployment both on the endpoints (in new transport layers and applications) as well as on middleboxes (run by network operators). This document summarizes the contributions to the workshop, provides an overview of the discussion at the workshop, as well as the outcomes and next steps identified by the workshop.

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

## Organization of this report

This workshop report summarizes the contributions to and discussions at the workshop, organized by topic. We starting with a summary of the current state of the Internet, and explore the incentives which have made it that way and the role of incentives in evolution. Many contributions were broadly split into two areas: middlebox measurement, classification, and approaches to defense against middlebox modification of packets; and approaches to support transport evolution. All accepted position papers are available at https://www.iab.org/activities/workshops/semi/.

The outcomes of the workshop are discussed in {{outcomes}}, and discuss progress after the workshop toward each of the identified work items as of the time of publication of this report.

# The Problem in Review

[EDITOR'S NOTE: This section is missing. The plan is to summarize introduction level-setting presentation and discussion from the transcript and slides. Note that this work follows from ITAT {{RFC7305}}. Note also previous attempts at trying to solve the middlebox side of this problem. Cite previous and current IETF efforts in the space: MIDCOM, NSIS, BEHAVE, TRAM. See the workshop page at https://www.iab.org/activities/workshops/semi/ for papers, slides, and transcripts in the meantime.]

# Incentives for Stack Ossification and Evolution

The current situation is, of course, the the result of a variety of processes, and the convergence of incentives for network operators, content providers, network equipment vendors, application developers, operating system developers, and end users....

[EDITOR'S NOTE: This section is missing. The plan is to summarize presentation and discussion focusing on incentives - both those leading to the problem and that could be leveraged for deployment of new transports - from the transcript and slides. See the workshop page at https://www.iab.org/activities/workshops/semi/ for papers and transcripts in the meantime.]

# The Role and Rule of Middleboxes

Paths as first-order things; path API.

[EDITOR'S NOTE: This section is missing. The plan is to summarize presentation and discussion focusing primarily on middlebox aspects of the problem (i.e., the Monday morning session and follow-up discussion Tuesday) from the transcript and slides. See the workshop page at https://www.iab.org/activities/workshops/semi/ for papers and transcripts in the meantime.]

# Evolving the Transport Layer

[EDITOR'S NOTE: This section is missing. The plan is to summarize presentation and discussion focusing primarily on transport evolution and evolvability (i.e., the Monday afternoon session and follow-up discussion Tuesday) from the transcript and slides. See the workshop page at https://www.iab.org/activities/workshops/semi/ for papers and transcripts in the meantime.]

# Outcomes

The SEMI workshop identified several areas for further work, outlined below:

## Minimal signaling for encapsulated transports

Assuming that a way forward for transport evolution in user space would
involve encapsulation in UDP datagrams, the workshop identified that it may be
useful to have a facility built atop UDP to provide minimal signaling of the
semantics of a flow that would otherwise be available in TCP: at the very
least, indications of first and last packets in a flow to assist firewalls and
NATs in policy decision and state maintenance. This facility could also
provide minimal application-to-path and path-to-application signaling, though
there was less agreement exactly what should or could be signaled here.

The workshop did note that, given the increasing deployment of encryption in the
Internet, this facility should cooperate with DTLS {{RFC6347}} in order to
selectively expose information about traffic flows where the transport headers
and payload themselves are encrypted.

To develop this concept further, it was decided to propose a non working group
forming BoF session, SPUD (Substrate Protocol for User Datagrams), at the IETF
92 meeting in March in Dallas. A draft on use cases
{{I-D.hardie-spud-use-cases}}, a prototype specification for a shim protocol
over UDP  {{I-D.hildebrand-spud-prototype}, and a separate specification of
the use of DTLS as a subtransport layer
{{I-D.huitema-tls-dtls-as-subtransport}} were prepared following discussions
at SEMI, and presented at the BoF.

Clear from discussion before and during the SPUD BoF, and drawing on
experience with previous endpoint-to-middle and middle-to-endpoint signaling
approaches, is that any selective exposure of traffic metadata outside a
relatively restricted trust domain must be declarative as opposed to
imperative, non-negotiated, and advisory. Each exposed parameter should also
be independently verifiable, so that each entity can assign its own trust to
other entities. Basic transport over the substrate must continue working even
if signaling is ignored or stripped, to support incremental deployment. These restrictions on vocabulary are discussed further in {{I-D.trammell-stackevo-newtea}}.

There was much interest in the room in continuing work on an approach like the
one under discussion. While it was relatively clear that the state of the
discussion and prototyping activity now is not yet mature enough for
standardization within an IETF working group, it is not clear in what venue
the work should continue.

Discussion contiunes on the spud mailing list (spud@ietf.org). The UDP shim layer prototype described by {{I-D.hildebrand-spud-prototype}}.

## Middlebox measurement

Discussion about the impairments caused by middleboxes quickly identified the
need to get more and better data about how prevelant certain types of
impairments are in the network. It doesn't make much sense, for instance, to
engineer complex workarounds for certain types of impairments into transport
protocols if those impairments are relatively rare. There are dedicated
measurement studies for certain types of impairment, but the workshop noted
that prevalence data might be available from error logs from TCP stacks and
applications on both clients and servers: these entities are in a position to
know when attempts to use particular transport features failed, providing an
opportunity to measure the network as a side effect of using it. Many clients
already have a feature for sending these bug reports back to their developers.
These present opportunities to bring data to bear on discussion and decisions
about protocol engineering in an Internet full of middleboxes.

The HOPS (How Ossified is the Protocol Stack) informal birds of a feather
session ("BarBoF") was held at the IETF 92 meeting in Dallas, to discuss
approaches to get aggregated data from these logs about potential middlebox
impairment, focusing on common data formats and issues of preserving end-user
privacy. While some discussion focused on aggregating impairment observations
at the network level, initial work will focus on making relative prevalence
information available on an Internet-wide scope. The first activity identified
has been to match the types of data required to answer questions relevant to
protocol engineering to the data that currently is or can easily be collected.

A mailing list (hops@ietf.org) has been established to continue discussion.

## Guidelines for middlebox design deployment

The workshop identified the potential to update {{RFC3234}} to provide guidelines...

[EDITOR'S NOTE: potential IAB document following from {{RFC3234}}]

## Additional Activities in the IETF and IAB

[EDITOR'S NOTE: tsvarea presentation on transport protocol extensibility, tsvarea presentation on general UDP encapsulation considerations, appsarea presentation on the use of TLS/DTLS as middlebox modification and inspection prevention approach.]

## Additional Activities in Other Venues

[EDITOR'S NOTE: informal liaison to ETSI NFV ]

# Security Considerations

This document presents no security considerations.

# Acknowledgments

The IAB thanks the SEMI Program Committee: Brian Trammell, Mirja Kuehlewind, Joe Hildebrand, Eliot Lear, Mat Ford, Gorry Fairhurst, and Martin Stiemerling. We additionally thank Prof. Dr. Bernhard Plattner of the Communication Systems Group at ETH for hosting the workshop, and the Internet Society for its support.

# Attendees

The following people attended the SEMI workshop:

Aaron Yi Ding, Aaron Falk, Barry Leiba, Benoit Donnet, Bob Briscoe, Brandon Williams, Ken Calvert, Costin Raiciu, Colin Perkins, David Black, Dave Thaler, Dan Wing, Felipe Huici, Mat Ford, Gorry Fairhurst, Russ Housely, Christian Huitema, Brian Trammell, Joe Hildebrand, Jana Ivengar, Lars Eggert, Eliot Lear, Marc Blanchet, Mary Barns, Michael Welzl, Mirja Kuehlewind, Martin Stiemerling, Miroslav Ponec, Erik Nordmark, Philipp Schmidt, Bernhard Plattner, Richard Barnes, Spencer Dawkins, Szilveszter Nadas, Ted Hardie, Dan Wing, and Xing Li. Additionally, Eric Rescorla and Stuart Cheshire contributed to the workshop but were unable to attend.
