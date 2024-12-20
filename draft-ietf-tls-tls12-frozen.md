---
title: "TLS 1.2 is in Feature Freeze"
abbrev: "tls1.2-frozen"
category: info

docname: draft-ietf-tls-tls12-frozen-latest
submissiontype: IETF
number:
date:
consensus: true
v: 3
area: "Security"
workgroup: "Transport Layer Security"
keyword:
 - TLS
 - features
venue:
  group: "Transport Layer Security"
  type: "Working Group"
  mail: "tls@ietf.org"
  github: "tlswg/tls12-frozen"

author:
-
  fullname: Rich Salz
  organization: Akamai Technologies
  email: rsalz@akamai.com
-
  fullname: Nimrod Aviram
  email: nimrod.aviram@gmail.com

normative:
  TLS12: RFC5246
  TLS13: RFC8446
  TLS13REG: I-D.draft-ietf-tls-rfc8447bis

informative:
  ML-KEM:
    target: https://csrc.nist.gov/pubs/fips/203/final
    title: "Module-Lattice-Based Key-Encapsulation Mechanism Standard"
    date: "August 13, 2024"
  ML-DSA:
    target: https://csrc.nist.gov/pubs/fips/204/final
    title: "Module-Lattice-Based Key Digital Signature Standard"
    date: "August 13, 2024"
  SLH-DSA:
    target: https://csrc.nist.gov/pubs/fips/205/final
    title: "Stateless Hash-Based Key-Digital Signature Standard"
    date: "August 13, 2024"
  PQC:
    target: https://csrc.nist.gov/projects/post-quantum-cryptography
    date: January, 2017
    title: Post-Quantum Cryptography
  CFRGSLIDES:
    target: https://www.ietf.org/proceedings/95/slides/slides-95-cfrg-4.pdf
    title: Post Quantum Secure Cryptography Discussion
    author:
      ins: D. McGrew
      name: David McGrew
  PQUIPWG:
    target: https://datatracker.ietf.org/wg/pquip/about/
    title: Post-Quantum Use in Protocols
  TLSWG:
    target: https://datatracker.ietf.org/wg/tls/about/
    title: Transport Layer Security

--- abstract

Use of TLS 1.3 is growing and fixes some known deficiencies in TLS 1.2.
This document specifies that outside of
urgent security fixes, new TLS Exporter Labels, or new
Application-Layer Protocol Negotiation (ALPN) Protocol IDs,
no changes will be approved for TLS 1.2.
This prescription does not pertain to DTLS (in any DTLS version); it pertains to
TLS only.

--- middle

# Introduction {#sec-reasons}

Use of TLS 1.3 {{TLS13}} is growing, and it
fixes most known deficiencies with TLS 1.2 {{TLS12}}, such as
encrypting more of the traffic so that it is not readable by outsiders and
removing most cryptographic primitives now considered weak. Importantly, TLS
1.3 enjoys robust security proofs.

Both versions have several extension points, so items like new cryptographic
algorithms, new supported groups (formerly "named curves"),  etc., can be
added without defining a new protocol. This document specifies that outside of
urgent security fixes, and the exceptions listed in {{iana}},
no changes will be approved for TLS 1.2.
This prescription does not pertain to DTLS (in any DTLS version); it pertains to
TLS only.

# Implications for post-quantum cryptography

Cryptographically relevant quantum computers, once available, will have a
huge impact on RSA, FFDH, and ECC which are currently used in TLS.
In 2016, the US National Institute of Standards and Technology started a
multi-year effort to standardize algorithms that will be "safe"
once quantum computers are feasible {{PQC}}. First IETF discussions happened
around the same time {{CFRGSLIDES}}.

In 2024 NIST released standards for {{ML-KEM}}, {{ML-DSA}}, and {{SLH-DSA}}.
While industry was waiting for NIST to finish standardization, the
IETF has had several efforts underway.
A working group was formed in early 2023 to work on use of PQC in IETF protocols,
{{PQUIPWG}}.
Several other working groups, including TLS {{TLSWG}},
are working on
drafts to support hybrid algorithms and identifiers, for use during a
transition from classic to a post-quantum world.

For TLS it is important to note that the focus of these efforts is exclusively
TLS 1.3 or later.
Put bluntly, post-quantum cryptography for
TLS 1.2 WILL NOT be supported (see {{iana}}) at any time and anyone wishing
to deploy post-quantum cryptography should expect to be using TLS 1.3.

# IANA Considerations {#iana}

IANA will stop accepting registrations for any TLS parameters {{TLS13REG}}
except for the following:

- TLS Exporter Labels
- TLS Application-Layer Protocol Negotiation (ALPN) Protocol IDs

Entries in any other TLS protocol registry should have an indication like
"For TLS 1.3 or later" in their entry.

--- back
