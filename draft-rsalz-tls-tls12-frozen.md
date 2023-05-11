---
title: "TLS 1.2 is Frozen"
abbrev: "tls1.2-frozen"
category: info

docname: draft-rsalz-tls-tls12-frozen-latest
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
  github: "richsalz/tls12-frozen"

author:
  fullname: Rich Salz
  organization: Akamai Technologies
  email: rsalz@akamai.com

normative:
  TLS12: RFC5246
  TLS13: RFC8446
  TLS13REG: RFC8447
  QUICTLS: RFC9001
  DNSTLS: RFC8310

informative:
  PQC:
    target: https://csrc.nist.gov/projects/post-quantum-cryptography
    date: January, 2017
    title: Post=Quantum Cryptography
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

TLS 1.2 is in widespread use and can be configured such that it provides good
security properties. TLS 1.3 is also in
widespread use and fixes some known deficiencies with TLS 1.2, such as
removing error-prone cryptographic primitives and encrypting more of the traffic
so that it is not readable by outsiders.

Both versions have several extension points, so items like new cryptographic
algorithms, new supported groups (formerly "named curves"),  etc., can be
added without defining a new protocol. This document specifies that TLS 1.2
is frozen: no new algorithms or extensions will be approved.

Further, TLS 1.3 use is widespread, and new protocols should require and
assume its existence.

--- middle

# Introduction

TLS 1.2 {{TLS12}} is in widespread use and can be configured such that it provides good
security properties. However, this protocol version suffers from several
deficiencies:

1. While application layer traffic is always encrypted, most of the handshake
messages are not encrypted. Therefore, the privacy provided is suboptimal.

2. The list of cryptographic primitives specified for the protocol, both in-use
primitives and deprecated ones, includes several primitives that were a source for
vulnerabilities throughout the years, such as RSA key exchange, CBC cipher suites,
and problematic finite-field Diffie-Hellman group negotiation.
See {{sec-considerations}} for elaboration.

3. The original protocol, as-is, does not provide security (cite Renegotiation
attack). Rather, some extensions are required to provide security.

In contrast, TLS 1.3 {{TLS13}} is also in
widespread use and fixes most known deficiencies with TLS 1.2, such as
encrypting more of the traffic so that it is not readable by outsiders and
removing most cryptographic primitives considered dangerous. Importantly, TLS
1.3 enjoys robust security proofs and provides excellent security as-is.

Both versions have several extension points, so items like new cryptographic
algorithms, new supported groups (formerly "named curves"),  etc., can be
added without defining a new protocol. This document specifies that TLS 1.2
is frozen: no new algorithms or extensions will be approved.

The reasons for freezing TLS 1.2 are as follows:

1. Specifying and deploying new extension points requires resources, both in WG
discussions and in deployment efforts. These resources are better spent on
TLS 1.3.

2. For deployments that require recent extension points, it is best to deploy
TLS 1.3. This not only allows using these extension points, but also improves
security and efficiency in general. (On the other hand, continuing to extend TLS
1.2 would have created a slight incentive to avoid upgrading to TLS 1.3.)

3. TLS 1.2 will eventually be deprecated. Experience shows that deprecating
cryptographic primitives and protocols can be laborious and require a long
timeline (cite David Benjamin's RWC talk, Ryan Sleevi's SHA1 history).
Among other advantages, the decision to freeze TLS 1.2 clearly communicates to
vendors and operators that they should upgrade to TLS 1.3, and hopefully this
will allow for an easier deprecation story when TLS 1.2 is eventually deprecated.

Further, TLS 1.3 use is widespread, and new protocols should require and
assume its existence.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

# Implications for post-quantum cryptography

Quantum computers, once available, will have a huge impact on TLS.
In 2016, the US National Institute of Standards and Technology started a
multi-year effort to standardize algorithms that will be "safe"
once quantum computers are feasible {{PQC}}. First IETF discussions happened
around the same time {{CFRGSLIDES}}.

While the industry is waiting for NIST to finish standardization, the
IETF has several efforts underway.
A working group was formed in early 2013 to work on use of PQC in IETF protocols,
{{PQUIPWG}}.
Several other working groups, including TLS {{TLSWG}},
are working on
drafts to support hybrid algorithms and identifiers, for use during a
transition from classic to a post-quantum world.

For TLS it is important to note that the focus of these efforts is TLS 1.3
or later.
TLS 1.2 is WILL NOT be supported (see {{iana}}).

# TLS Use by Other Protocols

Any new protocol that uses TLS MUST specify TLS 1.3 as its default.
For example, QUIC {{QUICTLS}} requires TLS 1.3 and specifies that endpoints
MUST terminate the connection if an older version is used.

If deployment considerations are a concern, the protocol MAY specify TLS 1.2 as
an additional, non-default option.
As a counter example, the Usage Profile for DNS over TLS {{DNSTLS}} specifies
TLS 1.2 as the default, while also allowing TLS 1.3.
For newer specifications that choose to support TLS 1.2, those preferences are
to be reversed.

# Security Considerations {#sec-considerations}

(TODO Nimrod) I suggest giving the following references as security considerations:
- RSA key exchange and DH: Refer to draft-obsolete-kex.
- CBC cipher suites: I'll cite papers.
- Renegotiation: Refer to RFC 5746.
- I can add a paragraph of further prominent attacks in TLS history, explain
  that most of these are prevented in TLS 1.3.
Please let me know if you'd like me to write that?

# IANA Considerations {#iana}

IANA will stop accepting registrations for any TLS parameters {{TLS13REG}}
except for the following:

- TLS Exporter Labels
- TLS Application-Layer Protocol Negotiation (ALPN) Protocol IDs

Entries in any other TLS protocol registry should have an indication like
"For TLS 1.3 or later" in their entry.


--- back

# Acknowledgments
{:numbered="false"}

None yet.
