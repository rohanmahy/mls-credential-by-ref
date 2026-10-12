---
title: "Credentials By-Reference in the Messaging Layer Security (MLS) Protocol"
abbrev: "MLS By-Ref Credentials"
category: info

docname: draft-mahy-mls-credential-by-ref-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: "Security"
workgroup: "Messaging Layer Security"
keyword:
 - credential type
 - smaller LeafNode
 - smaller Credential
 - MLS scaling
venue:
  group: "Messaging Layer Security"
  type: "Working Group"
  mail: "mls@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/browse/mls/"
  github: "rohanmahy/mls-credential-by-ref"
  latest: "https://rohanmahy.github.io/mls-credential-by-ref/draft-mahy-mls-credential-by-ref.html"

author:
 -
    fullname: Rohan Mahy
    organization:
    email: rohan.ietf@gmail.com

normative:

informative:


--- abstract

Credentials in MLS can grow to become quite large.
Many credentials appear repeatedly in several MLS groups.
This extension allows MLS credentials to be included by using a hash reference of the actual credential.


--- middle

# Introduction {#intro}

This new credential type allows an MLS LeafNode to contain a hash reference to another MLS credential.
This could be be communicated out-of-band or in another MLS group.


# Conventions and Definitions

{::boilerplate bcp14-tagged}

# Credential Format

~~~ tls
struct {
    CredentialType referenced_credential_type;
    HashReference referenced_credential_hash<V>;
} ByRefCredential;

by_ref = ByRefCredential;
~~~

The `referenced_credential_hash` is the hash of the entire Credential struct of the `referenced_credential_type`, using the hash function from the cipher suite of the KeyPackage or the MLS group.

The `referenced_credential_type` MUST NOT be a `by_ref` Credential type.


# Security Considerations {#security}

The security of this mechanism depends on the strength of the hash function's resistance to birthday attacks.

Clients implementing this extension need to consider that the lookup of credential hashes could potentially leak timing information.
Implementations must be careful that the scope of applicability (ex: the audience in a web token, or the domain in an X.509 certificate) is relevant to the context of the LeafNode (in an MLS group or in a KeyPackage).


# IANA Considerations {#iana}

This document requests IANA to add the following entries to the MLS Credential Types registry.
Please replace RFCXXXX with the RFC of this document

- Value: 0x0007 (suggested)
- Name: by_ref
- Recommended: Y
- Reference: RFCXXXX


--- back

