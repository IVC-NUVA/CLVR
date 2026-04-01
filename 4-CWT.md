## Embedding in a CBOR Web Token

The CLVR reuses a documented standard, [CBOR Web Token (CWT)](https://www.rfc-editor.org/rfc/rfc8392.html), that defines a common structure and attributes for CBOR encoded contents.

This structure is a CBOR map with some predefined numeric keys, published by IANA [here](https://www.iana.org/assignments/cwt/cwt.xhtml). The keys used in the CLVR content header are:

| Key value | Meaning                                     |
|-----------|---------------------------------------------|
| 1         | Issuer (string)                             |
| 4         | Expiration time (in Unix format)            |
| 6         | Issued at time (in Unix format)             |
| -260      | Health certificate (the [compacted content](3-Compacting.md)) |

The content header is thus as follows, followed by the HCERT content:

<table>
<tbody>
<tr><td rowspan=9 valign=top>Map(4)</td><td></td><td>A4</td></tr>
    <tr><td>1 (Issuer)</td><td>01</td></tr>
    <tr><td> => “SYA”</td><td>53 59 41</td></tr>
    <tr><td>4 (Expiration time)</td><td>04</td></tr>
    <tr><td>=> 2 556 054 000 (31/12/2050)</td><td>1A 98 5A 49 F0</td></tr>
    <tr><td>6 (Issued at)</td><td>06</td></tr>
    <tr><td>1 754 379 469 (05/08/2025)</td><td>1A 68 91 B4 CD</td></tr>
    <tr><td>-260 (HCERT)</td><td>39 01 03</td></tr>
	<tr><td>CBOR HCERT content</td><td>A4 53 76 ....</td></tr>
</tbody>
<table>


