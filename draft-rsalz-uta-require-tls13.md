---
title: "New Protocols Must Require TLS 1.3"
abbrev: "require-tls1.3"
category: info

docname: draft-rsalz-uta-require-tls13-latest
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
  TRIPLESHAKE:
    target: https://mitls.org/pages/attacks/3SHAKE
    title: Triple Handshakes Considered Harmful Breaking and Fixing Authentication over TLS
  RENEG1:
    title: Understanding the TLS Renegotiation Attack
    author:
      name: Eric Rescorla
    target: https://web.archive.org/web/20091231034700/http://www.educatedguesswork.org/2009/11/understanding_the_tls_renegoti.html
  RENEG2:
    title: Authentication Gap in TLS Renegotiation
    author:
      - name: Marsh Ray
    target: https://web.archive.org/web/20091228061844/http://extendedsubset.com/?p=8
  LUCKY13:
    title: "Lucky Thirteen: Breaking the TLS and DTLS record protocols"
    target: http://www.isg.rhul.ac.uk/tls/TLStiming.pdf
    author:
      - ins: N. J. Al Fardan
      - ins: K. G. Paterson
  LUCKY13FIX:
    title: "Systematic fuzzing and testing of TLS libraries"
    target: https://nds.rub.de/media/nds/veroeffentlichungen/2016/10/19/tls-attacker-ccs16.pdf
    author:
      name: Juraj Somorovsky
  CBCSCANNING:
    title: Scalable Scanning and Automatic Classification of TLS Padding Oracle Vulnerabilities
    target: https://www.usenix.org/system/files/sec19-merget.pdf
    author:
      - name: Robert Merget
      - name: Juraj Somorovsky
      - name: Nimrod Aviram
      - name: Craig Young
      - name: Janis Fliegenschmidt
      - name: Jörg Schwenk
      - name: Yuval Shavitt
  BEAST:
    title: "Here come the xor ninjas"
    target: http://www.hpcc.ecs.soton.ac.uk/dan/talks/bullrun/Beast.pdf
    author:
      - name: Thai Duong
      - name: Juliano Rizzo
  WEAKDH:
    title: "Imperfect forward secrecy: How Diffie-Hellman fails in practice"
    target: https://dl.acm.org/doi/pdf/10.1145/2810103.2813707
    author:
      - ins: D. Adrian
      - ins: K. Bhargavan
      - ins: Z. Durumeric
      - ins: P. Gaudry
      - ins: M. Green
      - ins: J. A.Halderman
      - ins: N. Heninger
      - ins: D. Springall
      - ins: E. Thomé
      - ins: L. Valenta
      - ins: B. VanderSloot
  FREAK:
    title: "A messy state of the union: Taming the composite state machines of TLS"
    target: https://inria.hal.science/hal-01114250/file/messy-state-of-the-union-oakland15.pdf
    author:
      - name: Benjamin Beurdouche
      - name: Karthikeyan Bhargavan
      - name: Antoine Delignat-Lavaud
      - name: Cédric Fournet
      - name: Markulf Kohlweiss
      - name: Alfredo Pironti
      - name: Pierre-Yves Strub
      - name: Jean Karim Zinzindohoue
  SLOTH:
    title: "Transcript collision attacks: Breaking authentication in TLS, IKE, and SSH"
    target: https://inria.hal.science/hal-01244855/file/SLOTH_NDSS16.pdf
    author:
      - name: Karthikeyan Bhargavan
      - name: Gaëtan Leurent

--- abstract

TLS 1.2 is in widespread use and can be configured such that it provides good
security properties. TLS 1.3 is also in
widespread use and fixes some known deficiencies with TLS 1.2, such as
removing error-prone cryptographic primitives and encrypting more of the traffic
so that it is not readable by outsiders.

Since TLS 1.3 use is widespread, new protocols must require and
assume its existence.
This prescription does not pertain to DTLS (in any DTLS version); it pertains to
TLS only.

--- middle

# Introduction {#sec-reasons}

TLS 1.2 {{TLS12}} is in widespread use and can be configured such that it provides good
security properties. However, this protocol version suffers from several
deficiencies:

1. While application layer traffic is always encrypted, most of the handshake
messages are not encrypted. Therefore, the privacy provided is suboptimal.
This is a protocol issue that cannot be addressed by configuration.

2. The list of cryptographic primitives specified for the protocol, both in-use
primitives and deprecated ones, includes several primitives that were a source for
vulnerabilities throughout the years, such as RSA key exchange, CBC cipher suites,
and problematic finite-field Diffie-Hellman group negotiation.
This deficiency may be addressed through proper configuration; however,
experience shows that configuration mistakes are common, especially when
deploying cryptography.
See {{sec-considerations}} for elaboration.

3. The original protocol, as-is, does not provide security {{RENEG1}},
{{RENEG2}}, {{TRIPLESHAKE}}. Rather, some extensions are required to provide
security.

In contrast, TLS 1.3 {{TLS13}} is also in
widespread use and fixes most known deficiencies with TLS 1.2, such as
encrypting more of the traffic so that it is not readable by outsiders and
removing most cryptographic primitives considered dangerous. Importantly, TLS
1.3 enjoys robust security proofs and provides excellent security as-is.

This document specifies that, since TLS 1.3 use is widespread, new protocols
must require and assume its existence.
This prescription does not pertain to DTLS (in any DTLS version); it pertains to
TLS only.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

# Implications for post-quantum cryptography

Cryptographically-relevant
quantum computers (CRQC), once available, will have a huge impact on TLS.
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
TLS 1.2 WILL NOT be supported (see {{iana}}).
This is one more reason for new protocols to default to TLS 1.3, where
post-quantum cryptography is expected to be supported.

# TLS Use by Other Protocols

Any new protocol that uses TLS MUST specify as its default TLS 1.3 (or a higher
TLS version, when one becomes stadardized).
For example, QUIC {{QUICTLS}} requires TLS 1.3 and specifies that endpoints
MUST terminate the connection if an older version is used.

If deployment considerations are a concern, the protocol MAY specify TLS 1.2 as
an additional, non-default option.
As a counter example, the Usage Profile for DNS over TLS {{DNSTLS}} specifies
TLS 1.2 as the default, while also allowing TLS 1.3.
For newer specifications that choose to support TLS 1.2, those preferences are
to be reversed.

# Security Considerations {#sec-considerations}

TLS 1.2 was specified with several cryptographic primitives and design choices
that have historically hindered its security. The purpose of this section is to
briefly survey several such prominent problems that have affected the protocol.
It should be noted, however, that TLS 1.2 can be configured securely; it is
merely much more difficult to configure it securely as opposed to using its
modern successor, TLS 1.3. See {{!RFC9325}} for a more thorough guide on the
secure deployment of TLS 1.2.

Firstly, the TLS 1.2 protocol, without any extension points, is vulnerable to
the renegotiation attack and the Triple Handshake attack. Broadly, these attacks
exploit the protocol's support for renegotiation in order to inject a prefix
chosen by the attacker into the plaintext stream. This is usually a devastating
threat in practice, that allows e.g. obtaining secret cookies in a web setting.
Refer to {{RENEG1}}, {{RENEG2}}, {{TRIPLESHAKE}} for elaboration. In light of
the above problems, {{!RFC5746}} specifies an extension that prevents this
category of attacks. To securely deploy TLS 1.2, either renegotiation must be
disabled entirely, or this extension must be present. Additionally, clients must
not allow servers to renegotiate the certificate during a connection.

Secondly, the original key exchange methods specified for the protocol, namely
RSA key exchange and finite field Diffie-Hellman, suffer from several
weaknesses. As before, to securely deploy the protocol, these key exchange
methods must be disabled.
Refer to draft-obsolete-kex for elaboration (TODO I guess we will anyway
wait for WGLC for draft-obsolete-kex, so no sense to temporarily refer to the
draft.)

Thirdly, symmetric ciphers which were widely-used in the protocol, namely RC4
and CBC cipher suites, suffer from several weaknesses. RC4 suffers from
exploitable biases in its key stream; see {{!RFC7465}}. CBC cipher suites have
been a source of vulnerabilities throughout the years. A straightforward
implementation of these cipher suites inherently suffers from the Lucky13 timing
attack {{LUCKY13}}. The first attempt to implement the cipher suites in
constant time introduced an even more severe vulnerability {{LUCKY13FIX}}.
There have been further similar vulnerabilities throughout the
years exploiting CBC cipher suites; refer to e.g. {{CBCSCANNING}}
for an example and a survey of similar works.

And lastly, historically the protocol was affected by several other attacks that
TLS 1.3 is immune to:
BEAST {{BEAST}}, Logjam {{WEAKDH}}, FREAK {{FREAK}}, and SLOTH {{SLOTH}}.

# IANA Considerations {#iana}

This document makes no requests to IANA.

--- back

# Acknowledgments
{:numbered="false"}

None yet.
