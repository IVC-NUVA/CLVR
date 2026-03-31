## Embedding in a CBOR Web Token

The CLVR reuses a documented standard, CBOR Web Token (CWT)[^5], that defines a common structure and attributes for CBOR encoded contents.

[^5]: <https://www.rfc-editor.org/rfc/rfc8392.html>

This structure is a CBOR map with some predefined numeric keys, published by IANA at <https://www.iana.org/assignments/cwt/cwt.xhtml>. The keys used in the CLVR content header are:

| Key value | Meaning                                     |
|-----------|---------------------------------------------|
| 1         | Issuer (string)                             |
| 4         | Expiration time (in Unix format)            |
| 6         | Issued at time (in Unix format)             |
| -260      | Health certificate (the [compacted content](3-Compacting.md)) |

The content header is thus as follows, followed by the HCERT content:

<table>
<tbody>
<tr><td rowspan=8 style="vertical-align:top">Map(4)</td><td></td><td>A4</td></tr>
    <tr><td>1 (key)</td><td>01</td></tr>
    <tr><td>“SYA” (value)</td><td>53 59 41</td></tr>
    <tr><td>4 (key)</td><td>04</td></tr>
    <tr><td>2 556 054 000 (31/12/2050) (value)</td><td>1A 98 5A 49 F0</td></tr>
    <tr><td>6 (key)</td><td>06</td></tr>
    <tr><td>1 754 379 469 (05/08/2025) (value)</td><td>1A 68 91 B4 CD</td></tr>
    <tr><td>-260 (key)</td><td>39 01 03</td></tr>	
</tbody>

<table>

The juxtaposition of the content header and the content form the CWT token.

