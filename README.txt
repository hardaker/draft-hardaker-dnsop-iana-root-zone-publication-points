



Domain Name System Operations                                  W. Kumari
Internet-Draft                                              Google, Inc.
Updates: RFC8806 (if approved)                               W. Hardaker
Intended status: Standards Track                USC/ISI and Google, Inc.
Expires: 20 July 2026                                            J. Reid
                                                                RTFM llp
                                                               G. Huston
                                                                   APNIC
                                                         16 January 2026


                Populating resolvers with the root zone.
     draft-hardaker-dnsop-iana-root-zone-publication-points-latest

Abstract

   DNS recursive resolvers implementing functionality like LocalRoot or
   similar services need to obtain the contents of the IANA DNS root
   zone on a regular basis.  This document describes a machine readable
   format for IANA to use when publishing a list of known sources that
   can be use for obtaining the contents of the IANA DNS Root Zone.

About This Document

   This note is to be removed before publishing as an RFC.

   Status information for this document may be found at
   https://datatracker.ietf.org/doc/draft-hardaker-dnsop-iana-root-zone-
   publication-points/.

   Discussion of this document takes place on the Domain Name System
   Operations Working Group mailing list (mailto:dnsop@ietf.org), which
   is archived at https://mailarchive.ietf.org/arch/browse/dnsop/.
   Subscribe at https://www.ietf.org/mailman/listinfo/dnsop/.

   Source for this draft and an issue tracker can be found at
   https://github.com/https://github.com/hardaker/draft-hardaker-dnsop-
   iana-root-zone-publication-points.

Status of This Memo

   This Internet-Draft is submitted in full conformance with the
   provisions of BCP 78 and BCP 79.

   Internet-Drafts are working documents of the Internet Engineering
   Task Force (IETF).  Note that other groups may also distribute
   working documents as Internet-Drafts.  The list of current Internet-
   Drafts is at https://datatracker.ietf.org/drafts/current/.

   Internet-Drafts are draft documents valid for a maximum of six months
   and may be updated, replaced, or obsoleted by other documents at any
   time.  It is inappropriate to use Internet-Drafts as reference
   material or to cite them other than as "work in progress."

   This Internet-Draft will expire on 20 July 2026.

Copyright Notice

   Copyright (c) 2026 IETF Trust and the persons identified as the
   document authors.  All rights reserved.

   This document is subject to BCP 78 and the IETF Trust's Legal
   Provisions Relating to IETF Documents (https://trustee.ietf.org/
   license-info) in effect on the date of publication of this document.
   Please review these documents carefully, as they describe your rights
   and restrictions with respect to this document.  Code Components
   extracted from this document must include Revised BSD License text as
   described in Section 4.e of the Trust Legal Provisions and are
   provided without warranty as described in the Revised BSD License.

Table of Contents

   1.  Introduction
   2.  IANA maintained list of root zone publication points
     2.1.  Root zone publication points
     2.2.  Publication point operational considerations
   3.  Operational Considerations
   4.  Security Considerations
   5.  IANA Considerations
   6.  References
     6.1.  Normative References
     6.2.  Informative References
   Acknowledgments
   Authors' Addresses

1.  Introduction

   DNS recursive resolvers implementing functionality like LocalRoot
   [draft-wkumari-dnsop-localroot-bcp] or similar services need to
   obtain the contents of the IANA DNS root zone on a regular basis.
   This document describes a machine readable format for IANA to use
   when publishing a list of known sources that can be use for obtaining
   the contents of the IANA DNS Root Zone.

2.  IANA maintained list of root zone publication points

   This list of IANA root zone data publication points available at TBD-
   URL may be used when downloading and refreshing the root zone data,
   as described in [draft-wkumari-dnsop-localroot-bcp].  Specifically,
   this IANA DNS root zone publication list MAY be used by the resolver
   software directly, or by the operating system a resolver is deployed
   on, or by a network operator when configuring a resolver.

   The contents of the IANA DNS root publication points file MUST
   verified as to its integrity as having come from IANA and MUST be
   verified as complete.

2.1.  Root zone publication points

   NOTE: this is but an example format that is expected to spur
   discussions within IETF working groups like DNSOP.  Whether this is a
   list in a simple line-delimited format like below or signed JSON or
   signed PGP or ... is subject to debate.

   The format of the IANA root zone data publication points file will
   consist of two parts, separated by a line containing four dashes and
   a newline ("----\n").  The top section of the file contain a newline
   delimited list of URLs [RFC2056].  The second section, following the
   line containing four dashes, will contain a cryptographic checksum or
   signature.  Note that the format of this file applies to the IANA
   maintained list of root zone publication points, but may or may not
   be a format used by other publication point aggregation lists.

   URLs in the list may include any protocol capable of transferring DNS
   zone data, including HTTPS [RFC9110], AXFR
   [draft-hardaker-dnsop-dns-xfr-scheme], XoT
   [draft-hardaker-dnsop-dns-xfr-scheme], etc.

   Any URLs that reference an unknown transfer protocol SHOULD be
   discarded.  If after filtering the list there are no acceptable list
   elements left, the resolver MUST revert to using regular DNS queries
   to the IANA root zone instead of operating as a LocalRoot.

   The first line of the cryptograhpic checksum section will contain a
   checksum or signature type string specifying what the remaining lines
   in the checksum or signature section will contain.

   An minimal example publication point file, containing only a single
   AXFR publication point of b.root-servers.net:

   axfr:b.root-servers.net/.
   ----
   SHA256
   67d687eb21e59321dbb8115c51d1b4ddbd6634362859d130ed77b47a4410656c

2.2.  Publication point operational considerations

   Implementations SHOULD optimize retrieval to minimize impacts on the
   server.  Because the list is not expected to change frequently,
   implementations SHOULD refrain from querying the IANA source more
   than once a week.

3.  Operational Considerations

   TBD

4.  Security Considerations

   TBD

5.  IANA Considerations

   TBD: describe the request for IANA to support a list of root server
   publication points at TBD-URL.

6.  References

6.1.  Normative References

   [BCP237]   Best Current Practice 237,
              <https://www.rfc-editor.org/info/bcp237>.
              At the time of writing, this BCP comprises the following:

              Hoffman, P., "DNS Security Extensions (DNSSEC)", BCP 237,
              RFC 9364, DOI 10.17487/RFC9364, February 2023,
              <https://www.rfc-editor.org/info/rfc9364>.

   [RFC1982]  Elz, R. and R. Bush, "Serial Number Arithmetic", RFC 1982,
              DOI 10.17487/RFC1982, August 1996,
              <https://www.rfc-editor.org/rfc/rfc1982>.

   [RFC4033]  Arends, R., Austein, R., Larson, M., Massey, D., and S.
              Rose, "DNS Security Introduction and Requirements",
              RFC 4033, DOI 10.17487/RFC4033, March 2005,
              <https://www.rfc-editor.org/rfc/rfc4033>.

   [RFC8198]  Fujiwara, K., Kato, A., and W. Kumari, "Aggressive Use of
              DNSSEC-Validated Cache", RFC 8198, DOI 10.17487/RFC8198,
              July 2017, <https://www.rfc-editor.org/rfc/rfc8198>.

   [RFC8499]  Hoffman, P., Sullivan, A., and K. Fujiwara, "DNS
              Terminology", RFC 8499, DOI 10.17487/RFC8499, January
              2019, <https://www.rfc-editor.org/rfc/rfc8499>.

   [RFC8806]  Kumari, W. and P. Hoffman, "Running a Root Server Local to
              a Resolver", RFC 8806, DOI 10.17487/RFC8806, June 2020,
              <https://www.rfc-editor.org/rfc/rfc8806>.

   [RFC8976]  Wessels, D., Barber, P., Weinberg, M., Kumari, W., and W.
              Hardaker, "Message Digest for DNS Zones", RFC 8976,
              DOI 10.17487/RFC8976, February 2021,
              <https://www.rfc-editor.org/rfc/rfc8976>.

6.2.  Informative References

   [BIND-MIRROR]
              "BIND 9 Mirror Zones", n.d.,
              <https://bind9.readthedocs.io/en/stable/
              reference.html#namedconf-statement-type%20mirror>.

   [CACHEME]  "Cache Me If You Can: Effects of DNS Time-to-Live", n.d.,
              <https://ant.isi.edu/~johnh/PAPERS/Moura19b.pdf>.

   [DNEROOTNAMES]
              "NoError vs NxDomain by-week", n.d.,
              <https://rssac002.root-servers.org/rcode_0_v_3.html>.

   [draft-hardaker-dnsop-dns-xfr-scheme]
              "The DNS XFR URI Schemes", n.d.,
              <https://datatracker.ietf.org/doc/draft-hardaker-dnsop-
              dns-xfr-scheme/>.

   [draft-hardaker-dnsop-root-zone-publication-list-guidelines]
              "Guidelines for IANA DNS Root Zone Publication List
              Providers", n.d.,
              <https://raw.githubusercontent.com/hardaker/draft-
              hardaker-dnsop-root-zone-publication-list-
              guidelines/refs/heads/main/draft-hardaker-dnsop-root-zone-
              publication-list-guidelines.md>.

   [draft-wkumari-dnsop-localroot-bcp]
              "Populating resolvers with the root zone", n.d.,
              <https://datatracker.ietf.org/doc/draft-wkumari-dnsop-
              localroot-bcp/>.

   [KNOT-PREFILL]
              "Knot Resolver Prefill", n.d., <https://knot-
              resolver.readthedocs.io/en/stable/modules-prefill.html>.

   [LOCALROOTPRIVACY]
              "Analyzing and mitigating privacy with the DNS root
              service", n.d., <http://ant.isi.edu/~hardaker/
              papers/2018-02-ndss-analyzing-root-privacy.pdf>.

   [NOROOTS]  "On Eliminating Root Nameservers from the DNS", n.d.,
              <https://www.icir.org/mallman/pubs/All19b/All19b.pdf>.

   [QNAMEMIN] "DNS Query Privacy", n.d.,
              <https://www.potaroo.net/ispcol/2019-08/qmin.html>.

   [RFC2056]  Denenberg, R., Kunze, J., and D. Lynch, "Uniform Resource
              Locators for Z39.50", RFC 2056, DOI 10.17487/RFC2056,
              November 1996, <https://www.rfc-editor.org/rfc/rfc2056>.

   [RFC5936]  Lewis, E. and A. Hoenes, Ed., "DNS Zone Transfer Protocol
              (AXFR)", RFC 5936, DOI 10.17487/RFC5936, June 2010,
              <https://www.rfc-editor.org/rfc/rfc5936>.

   [RFC7706]  Kumari, W. and P. Hoffman, "Decreasing Access Time to Root
              Servers by Running One on Loopback", RFC 7706,
              DOI 10.17487/RFC7706, November 2015,
              <https://www.rfc-editor.org/rfc/rfc7706>.

   [RFC7766]  Dickinson, J., Dickinson, S., Bellis, R., Mankin, A., and
              D. Wessels, "DNS Transport over TCP - Implementation
              Requirements", RFC 7766, DOI 10.17487/RFC7766, March 2016,
              <https://www.rfc-editor.org/rfc/rfc7766>.

   [RFC9110]  Fielding, R., Ed., Nottingham, M., Ed., and J. Reschke,
              Ed., "HTTP Semantics", STD 97, RFC 9110,
              DOI 10.17487/RFC9110, June 2022,
              <https://www.rfc-editor.org/rfc/rfc9110>.

   [RFC9156]  Bortzmeyer, S., Dolmans, R., and P. Hoffman, "DNS Query
              Name Minimisation to Improve Privacy", RFC 9156,
              DOI 10.17487/RFC9156, November 2021,
              <https://www.rfc-editor.org/rfc/rfc9156>.

   [UNBOUND-AUTH-ZONE]
              "Unbound Auth Zone", n.d.,
              <https://nlnetlabs.nl/documentation/unbound>.

Acknowledgments

   TBD

Authors' Addresses

   Warren Kumari
   Google, Inc.
   Email: warren@kumari.net


   Wes Hardaker
   USC/ISI and Google, Inc.
   Email: ietf@hardakers.net


   Jim Reid
   RTFM llp
   St Andrews House
   382 Hillington Road, Glasgow Scotland
   G51 4BL
   United Kingdom
   Email: jim@rfc1035.com


   Geoff Huston
   APNIC
   6 Cordelia St
   South Brisbane  QLD 4101
   Australia
   Email: gih@apnic.net
