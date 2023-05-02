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
  github: "richsalz/draft-tls12-frozen"

author:
  fullname: Rich Salz
  organization: Akamai Technologies
  email: rsalz@akamai.com

normative:
  RFC5246:
  RFC8446:
  RFC8447:

informative:


--- abstract

TLS 1.2 is in widespread use and can be configured in a secure manner: there
is nothing wrong with it. TLS 1.3 is also in widespread use and fixes some
known deficiencies with TLS 1.2, such as encrypting more of the traffic so
that it is not readable by outsiders.

Both versions have several extension points, so items like new cryptographic
algorithms, new elliptic curves, etc., can be added without defining a
new protocol. This document specifies that TLS 1.2 is frozen: no new algorithms
or extensions will be approved, and new protocols should require and assume
TLS 1.3.

--- middle

# Introduction

TLS 1.2 is in widespread use and can be configured in a secure manner: there
is nothing wrong with it. TLS 1.3 is also in widespread use and fixes some
known deficiencies with TLS 1.2, such as encrypting more of the traffic so
that it is not readable by outsiders.

Both versions have several extension points, so items like new cryptographic
algorithms, new elliptic curves, etc., can be added without defining a
new protocol. This document specifies that TLS 1.2 is frozen: no new algorithms
or extensions will be approved, and new protocols should require and assume
TLS 1.3.

# Conventions and Definitions

{::boilerplate bcp14-tagged}


# Security Considerations

Security considerations are discussed throughout this document.

# IANA Considerations

IANA will stop accepting registrations for TLS 1.2 for any of the following
tables:
>

New ciphers, etc., will have the note "For TLS 1.3 or later" in their entry.


--- back

# Acknowledgments
{:numbered="false"}

None yet.
