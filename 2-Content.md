# CLVR Content

## CLVR logical content

Given the above requirements, the CLVR must then contain:

-   Minimal patient identity traits: name, given name and date of birth.
-   For each administered vaccine:
    -   used vaccine product,
    -   date of administration,
    -   reference to an authoritative record, identified by a national registry, a repository of authoritative records within this registry and a local index in this repository.
-   A digital signature of the above contents

## JSON representation
The CLVR content is encoded in a binary compacted object but for convenience can be represented as a JSON structure with the following content.

| L1  | L2  | Card. | Type   | Example             | Comment                                         |
|-----|-----|-------|--------|---------------------|-------------------------------------------------|
| ver |     | 1..1  | string | 1.0.0               | Version of the structure                        |
| nam |     | 1..1  |        |                     | Basic identity traits                           |
|     | fnt | 1..1  | string | DOE                 | Name                                            |
|     | gnt | 1..1  | string | John                | First or usual given name                       |
| dob |     | 1..1  | date   | 2017-07-19          | Date of birth                                   |
| pid |     | 0..1  |        |                     | Optional digital identifier for the person      |
|     | oid | 1..1  | string | 1.2.250.1.213.1.4.8 | Object identifier for the identification scheme |
|     | id  | 1..1  | string | 1630777186051       | Person identifier within the scheme             |
| v   |     | 0..\* |        |                     | Vaccine administration records                  |
|     | reg | 1..1  | string | LUX                 | 2 to 6 letters code for a registry              |
|     | rep | 1..1  | int    | 5                   | Index for a repository in a registry            |
|     | i   | 1..1  | int    | 1296                | Reference within a repository for a given date  |
|     | a   | 1..1  | int    | 1386                | Age in days when the vaccine was administered   |
|     | mp  | 1..1  | int    | 29                  | NUVA code for the vaccine (here REPEVAX)        |

This structure is compacted in [CBOR](3-Compacting.md), then wrapped into a [CBOR Web Token](4-CWT.md) precising some metadata, itself wrapped into a [COSE](5-Signing.md) signed envelope.

## Example data

In this document, the CLVR content will be exemplified using the data found in the [EVC generator demonstrator](https://github.com/EUVABECO/EVC-generator).

<table>
    <tbody>
        <tr><td rowspan=3>Identity</td><td colspan = 2>Name</td><td>DOE</td></tr>
            <tr><td colspan = 2>Given name</td><td>John</td></tr>
            <tr><td colspan = 2>Date of birth</td><td>2017-07-19</td></tr>
		</tr>
        <tr><td rowspan=5>Vaccine[1]</td><td colspan=2>Administration date><td>2021-05-05</td></tr>
		    <tr><td colspan=2>Product</td><td>REPEVAX</td></tr>
			<tr><td rowspan=3>Master record</td><td>Registry</td><td>FRA</td></tr>
			    <tr><td>Repository</td><td>36</td></tr>
			    <tr><td>Index</td><td>1245</td></tr>
        <tr><td rowspan=5>Vaccine[2]</td><td colspan=2>Administration date><td>2022-03-03</td></tr>
		    <tr><td colspan=2>Product</td><td>QDENGA</td></tr>
			<tr><td rowspan=3>Master record</td><td>Registry</td><td>FRA</td></tr>
			    <tr><td>Repository</td><td>36</td></tr>
			    <tr><td>Index</td><td>987</td></tr>						
    </tbody>
</table>

# Overall structure

The CLVR is encoded as a COSE object (described in the below chapters). It consists of:

-   A [signature characterization header](5-Signing.md#signed-structure)
-   A [content header](4-CWT.md)
-   The [actual content](3-Compacting.md), consisting of the patient identity traits and the administered vaccines
-   The [signature value](5-Signing.md#computing-the-signature)

The diagram below presents this structure with the typical size in bytes of the different sections (before compression).
<table>
<thead style="font-weight:bold">
<tr><td></td><td></td><td>Identity</td><td colspan=8>Vaccines</td><td>Signature</td></tr>
</thead>
<tbody>
<tr><td>30</td><td>24</td><td>110</td><td>31</td><td>31</td><td>31</td><td>31</td><td>31</td><td>31</td><td>31</td><td>31</td><td>56</td></tr>
</tbody>
</table>


