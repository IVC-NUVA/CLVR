# Signing

## The COSE format

[CBOR Object Signing and Encryption (COSE)](https://www.rfc-editor.org/rfc/rfc9052.html>) is a further CBOR wrapper around the CWT token.

It is used in CLVR only for the purpose of signing, encryption is not needed.

## Signed structure

The signed content is formed by:

-   A tag stating that it is a COSE object
-   A signature header, specifying the algorithm and the signing key identifier.
-   The signed data, here the CWT token.
-   The signature value.

The attributes of the signature in the signature header are represented with maps, with their keys defined by IANA [here](https://www.iana.org/assignments/cose/cose.xhtml). The following attributes are used in the CLVR.


| Key value | Meaning        |
|-----------|----------------|
| 1         | COSE algorithm |
| 4         | Key identifier |

The COSE algorithm values are taken from the COSE algorithms listed in the same page. The current implementation uses the ES256 algorithm, encoded as -7. This algorithm produces a 64 bytes signature that will be appended at the end of the object.

Contrary to many signing mechanisms, the public part of the signing key is not embedded into the signed document, but referenced by a key identifier, that will be used to fetch the public key from a commonly agreed keystore (normally exposed by the WHO Global Digital Health Certification Network (GDHCN). In the current implementation of CLVR, the keystore is exposed as a JWKS resource [here](https://keys.euvabeco.eu/.well-known/jwks.json).

Since the content of the data and the selected algorithm could have been encrypted, these sections are represented as byte arrays.

The overall object structure is thus finally:
<table>
<tbody>
<tr><td colspan=4>Tag (18) - COSE object</td><td>D2</td></tr>
<tr><td rowspan=12 valign=top>Array(4)</td><td colspan=3></td><td>84</td></tr>    
    <tr><td rowspan=4 valign=top>Bytes(3)</td><td colspan=2></td><td>43</td></tr>
        <tr><td rowspan=3 style="vertical-align:top">Map(1)</td><td></td><td>A1</td></tr>
		<tr><td>1(COSE algorithm)</td><td>01</td></tr>
		    <tr><td>-7 (ES256)</td><td>26</td></tr>
    <tr><td rowspan=3 valign=top>Map(1)</td><td colspan=2></td><td>A1</td></tr>
    <tr><td colspan=2>4(Key identifier)</td><td>04</td></tr>
        <tr><td colspan=2>"SYA25A" (id. in keystore)</td><td>46 53 59 41 32 35 41</td></tr>
    <tr><td rowspan=2 valign=top>Bytes (XX XX)</td><td colspan=2></td><td>59 XX XX</td></tr>
        <tr><td colspan=2>CWT content</td><td>...</td></tr>
    <tr><td rowspan=2 valign=top>Bytes (64)</td><td colspan=2></td><td>58 40</td></tr>
        <tr><td colspan=2 >Signature x and y</td><td>...</td></tr>
</tbody>
</table>

## Computing the signature

The signature is obtained with the given algorithm by signing not the CWT content itself, but a temporary CBOR object consisting of:

-   A fixed string “Signature1”
-   The Bytes object representing the signature algorithm
-   Optional extra parameters for the algorithm (none with ES256)
-   The CWT.

<table>
<tbody>
<tr><td rowspan=5 valign=top>Array(4)</td><td></td><td>84</td></tr>
    <tr><td>"Signature1"</td><td>6A 53 69 67 6E 61 74 75 72 65 31</td></tr>
	<tr><td>Algorithm</td><td>43 A1 01 26</td></tr>
	<tr><td>Void bytes string</td><td>40</td></tr>
	<tr><td>CWT</td><td>59 XX XX A4 01 53 59 41 ….   </td></tr>
</tbody>
</table>

Signing consists in computing a footprint of this content with SHA256, then obtaining its signature with the ECDSA private key. Explaining elliptic curve encryption goes beyond the scope of this document, yet [tools](https://emn178.github.io/online-tools/ecdsa/sign/) and libraries are widely available. They should be configured using the secp256r1 elliptic curve.

With this algorithm, the resulting signature consists of two 32 bytes terms, usually named x and y. They just have to be included sequentially into the 64 bytes signature field.

Depending upon the used signing library, it may be necessary to extract them from a returned composite object in DER format.

