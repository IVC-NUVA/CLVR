# CLVR - Compact Lifetime Vaccination Record

The Compact Lifetime Vaccination Record (CLVR, pronounced Clover) is a digital format for representing all vaccines received by a person in its lifetime, encodable as a QR Code.  

![](media/84c3df7bf0b371b63dd15e98aea9ccf5.png)

It relies upon different standards, each with a larger purpose and a comprehensive technical documentation.

This documentation consolidates the necessary information from these different standards to facilitate the work of implementers.

It consists of:
1. [Objectives of CLVR](1-Objectives.md)
1. [Content of a CLVR](2-Content.md)
1. [Compacting as a CBOR binary object](3-Compacting.md)
1. [Embedding in a CBOR Web Token](4-CWT.md)
1. [Signing the CWT](5-Signing.md)
1. [Compressing the signed object](6-Compressing.md)
1. [QR Code representation](7-QRCode.md)
1. [Standardized layout](8-Layout.md)

CLVR demonstrators have been implemented and shared on GitHub:

- [in Python](https://github.com/EUVABECO/EVC-generator), converting  both ways between a FHIR bundle and a CLVR.
- [in Ruby](https://github.com/EUVABECO/signer), as a backend server able to encode and decode CLVR.
- [in Java](https://github.com/cerbeor/ips-to-clvr), creating a CLVR from an HL7 International Patient Summary.
