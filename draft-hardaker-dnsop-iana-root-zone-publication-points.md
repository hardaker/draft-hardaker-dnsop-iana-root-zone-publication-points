---
title: "An IANA root zone publication source list format"
abbrev: "IANA root zone publication point list format"
category: std

docname: draft-hardaker-dnsop-root-zone-publication-points-latest
submissiontype: IETF
consensus: true
v: 3
area: "Operations and Management"
workgroup: "Domain Name System Operations"
keyword:
 - DNS
 - DNSSEC
 - zone cut
 - delegation
 - referral
updates: RFC8806
venue:
  group: "Domain Name System Operations"
  type: "Working Group"
  mail: "dnsop@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/browse/dnsop/"
  github: "https://github.com/hardaker/draft-hardaker-dnsop-root-zone-publication-points"

author:
  -
    fullname: Wes Hardaker
    organization: Google, Inc.
    email: ietf@hardakers.net

normative:
  BCP237:
  RFC4033:  # DNSSEC
  RFC8976:  # ZONEMD

informative:
  RFC5936:  # DNS Zone Transfer
  RFC7766:  # DNS Transport over TCP
  RFC9110:  # HTTP Semantics and Methods

  draft-hardaker-dnsop-dns-xfr-scheme:
    title: The DNS XFR URI Schemes
    target: https://datatracker.ietf.org/doc/draft-hardaker-dnsop-dns-xfr-scheme/
  draft-hardaker-dnsop-root-zone-publication-list-guidelines:
    title: Guidelines for IANA DNS Root Zone Publication List Providers
    target: https://datatracker.ietf.org/doc/draft-hardaker-dnsop-root-zone-publication-list-guidelines
  draft-wkumari-dnsop-localroot-bcp:
    title: Populating resolvers with the root zone
    target: https://datatracker.ietf.org/doc/draft-wkumari-dnsop-localroot-bcp/
  NOROOTS:
    title: On Eliminating Root Nameservers from the DNS
    target: https://www.icir.org/mallman/pubs/All19b/All19b.pdf

--- abstract

This document describes a machine readable format to be used by IANA to publish
a list of sources where the contents of the IANA DNS Root Zone may be fetched
from.

--- middle

# Introduction

DNS recursive resolvers implementing functionality such as LocalRoot
{{draft-wkumari-dnsop-localroot-bcp}} need to obtain the contents of
the IANA DNS root zone on a regular basis. This document describes a machine
readable format that can be used by IANA to publish a list of sources that can be
used for obtaining the contents of the IANA DNS Root Zone.

# IANA maintained list of root zone publication points  {#iana-root-zone-list}

The list of IANA root zone data publication points, available at TBD-URL, may
be used to discover where the IANA root zone data can be fetched from.

It is expected that this will be used as described in
{{draft-wkumari-dnsop-localroot-bcp}}, and may be used by the resolver software
directly, or by the operating system a resolver is deployed on, or by a network
operator when configuring a resolver.

The contents of the IANA DNS root publication points file MUST be verifiable as
to its integrity as having come from IANA and MUST be verifiable as being
complete.


## Root zone publication points

NOTE: this is but an example format that is expected to spur
discussions within IETF working groups like DNSOP.  Whether this is a
list in a simple line-delimited format like below or signed JSON or
signed PGP or ... is subject to debate.

The IANA root zone data publication points file is structured into two distinct
segments, divided by a line consisting of four dashes followed by a newline
("----\n"). The first segment contains a list of URLs, one per line. The second
segment provides a signature or cryptographic checksum.

While this specific format was originally designed for IANA's maintained list
of root zone publication points, it may also be used by other publication point
aggregation lists.

The list may include URLs using any protocol capable of transferring DNS zone
data, including HTTPS {{RFC9110}}, AXFR
{{draft-hardaker-dnsop-dns-xfr-scheme}}, XoT
{{draft-hardaker-dnsop-dns-xfr-scheme}}, etc.

Each URL MUST occur on its own line.  Lines beginning with the "#" character are
considered comments and MUST be ignored.  Leading and trailing whitespace on
each line SHOULD be ignored.

Any URLs that reference an unknown transfer protocol in the LocalRoot
implementations SHOULD be discarded.  If after filtering the list
there are no acceptable list
elements left, the resolver MUST revert to using regular DNS queries
to the IANA root zone instead of operating as a LocalRoot.

The first line of the cryptographic checksum section will contain a
checksum or signature type string specifying what the remaining lines
in the checksum or signature section will contain.
(Ed note: this section is underspecified.  We expect to
refine this as we get feedback from the working group.)

A minimal example publication point file, containing a single
AXFR publication point with a target of b.root-servers.net:

~~~~
http://www.internic.net/domain/root.zone
https://www.internic.net/domain/root.zone
axfr:b.root-servers.net/.
----
SHA256
fba70cbd347741662a3e6c27056d0b8be07b65085633891407e24731c6736307
~~~~

Future note: this should eventually be a signature from an identity,
regardless of format, that can be traced back to IANA being the
authoritative publisher and not just a simple checksum.

# Operational Considerations

Implementations SHOULD optimize retrieval to minimize impacts on the
server.  Because the list is not expected to change frequently,
implementations SHOULD refrain from querying IANA's source more than
once a week.

TBD

# Security Considerations

It is critical that LocalRoot implementations (or other any code
bases) making use of the publication point list format described in
this document verify the contents using the encoded checksum to ensure
it has not been tampered with.

TBD

# IANA Considerations

TBD: describe the request for IANA to support a list of root server
publication points at TBD-URL.

--- back


# Acknowledgments
{:numbered="false"}

TBD
