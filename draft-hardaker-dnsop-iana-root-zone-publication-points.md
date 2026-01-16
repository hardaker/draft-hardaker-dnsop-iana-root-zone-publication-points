---
title: "A format for publishing a list of sources of IANA root zone data"
abbrev: "IANA root zone publication points"
category: std

docname: draft-hardaker-dnsop-iana-root-zone-publication-points-latest
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
  github: "https://github.com/hardaker/draft-hardaker-dnsop-iana-root-zone-publication-points"

author:
  -
    fullname: Wes Hardaker
    organization: USC/ISI and Google, Inc.
    email: ietf@hardakers.net

normative:
  BCP237:
  RFC1982:   # SOA math
  RFC4033:   # DNSSEC
  RFC8198:   # Aggressive Use of DNSSEC-Validated Cache
  RFC8499:   # DNS Terminology
  RFC8806:   # DNS Zone Transfers over HTTPS
  RFC8976:   # DNS over HTTPS (DoH) Usage Profiles

informative:
  RFC5936:  # DNS Zone Transfer
  RFC7766:  # DNS Transport over TCP
  RFC7706:  # DNS Local Root
  RFC9156:  # QNAME Minimisation
  RFC9110:  # HTTP Semantics and Methods

  BIND-MIRROR:
    title: "BIND 9 Mirror Zones"
    target: https://bind9.readthedocs.io/en/stable/reference.html#namedconf-statement-type%20mirror
  UNBOUND-AUTH-ZONE:
    title: "Unbound Auth Zone"
    target: https://nlnetlabs.nl/documentation/unbound
  KNOT-PREFILL:
    title: "Knot Resolver Prefill"
    target: https://knot-resolver.readthedocs.io/en/stable/modules-prefill.html
  QNAMEMIN:
    title: DNS Query Privacy
    target: https://www.potaroo.net/ispcol/2019-08/qmin.html
  LOCALROOTPRIVACY:
    title: Analyzing and mitigating privacy with the DNS root service
    target: http://ant.isi.edu/~hardaker/papers/2018-02-ndss-analyzing-root-privacy.pdf
  CACHEME:
    title: "Cache Me If You Can: Effects of DNS Time-to-Live"
    target: https://ant.isi.edu/~johnh/PAPERS/Moura19b.pdf
  draft-hardaker-dnsop-dns-xfr-scheme:
    title: The DNS XFR URI Schemes
    target: https://datatracker.ietf.org/doc/draft-hardaker-dnsop-dns-xfr-scheme/
  draft-hardaker-dnsop-root-zone-publication-list-guidelines:
    title: Guidelines for IANA DNS Root Zone Publication List Providers
    target: https://raw.githubusercontent.com/hardaker/draft-hardaker-dnsop-root-zone-publication-list-guidelines/refs/heads/main/draft-hardaker-dnsop-root-zone-publication-list-guidelines.md
  draft-wkumari-dnsop-localroot-bcp:
    title: Populating resolvers with the root zone
    target: https://datatracker.ietf.org/doc/draft-wkumari-dnsop-localroot-bcp/
  NOROOTS:
    title: On Eliminating Root Nameservers from the DNS
    target: https://www.icir.org/mallman/pubs/All19b/All19b.pdf
  DNEROOTNAMES:
    title: NoError vs NxDomain by-week
    target: https://rssac002.root-servers.org/rcode_0_v_3.html

--- abstract

This document describes a machine readable format to be used by IANA to publish
a list of sources where the contents of the IANA DNS Root Zone may be fetched
from.

--- middle

# Introduction

DNS recursive resolvers implementing functionality such as LocalRoot
{{draft-wkumari-dnsop-localroot-bcp}} or similar need to obtain the contents of
the IANA DNS root zone on a regular basis. This document describes a machine
readable format to be used by the IANA to publish a list of sources that can be
used for obtaining the contents of the IANA DNS Root Zone.

# IANA maintained list of root zone publication points  {#iana-root-zone-list}

The list of IANA root zone data publication points, available at TBD-URL, may
be used to discover where the IANA root zone data can be fetched from.

It is expected that this will be used as described in
{{draft-wkumari-dnsop-localroot-bcp}}, and may be used by the resolver software
directly, or by the operating system a resolver is deployed on, or by a network
operator when configuring a resolver.

The contents of the IANA DNS root publication points file MUST be verified as
to its integrity as having come from IANA and MUST be verified as being
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

While this specific format was originally designed for the IANA maintained list
of root zone publication points, it may also be used by other publication point
aggregation lists.

URLs in the list may include any protocol capable of transferring DNS zone
data, including HTTPS {{RFC9110}}, AXFR
{{draft-hardaker-dnsop-dns-xfr-scheme}}, XoT
{{draft-hardaker-dnsop-dns-xfr-scheme}}, etc.

Each URL MUST be on its own line.  Lines beginning with the "#" character are
considered comments and MUST be ignored.  Leading and trailing whitespace on
each line MUST be ignored.

Any URLs that reference an unknown transfer protocol SHOULD be
discarded.  If after filtering the list there are no acceptable list
elements left, the resolver MUST revert to using regular DNS queries
to the IANA root zone instead of operating as a LocalRoot.

The first line of the cryptographic checksum section will contain a
checksum or signature type string specifying what the remaining lines
in the checksum or signature section will contain.
(Ed note: We know that this section is underspecified.  We expect to
refine this as we get feedback from the working group.)

An minimal example publication point file, containing only a single
AXFR publication point of b.root-servers.net:

~~~~
axfr:b.root-servers.net/.
----
SHA256
67d687eb21e59321dbb8115c51d1b4ddbd6634362859d130ed77b47a4410656c
~~~~

## Publication point operational considerations

Implementations SHOULD optimize retrieval to minimize impacts on the
server.  Because the list is not expected to change frequently,
implementations SHOULD refrain from querying the IANA source more than
once a week.

# Operational Considerations

TBD

# Security Considerations

TBD

# IANA Considerations

TBD: describe the request for IANA to support a list of root server
publication points at TBD-URL.

--- back


# Acknowledgments
{:numbered="false"}

TBD
