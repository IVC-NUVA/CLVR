# Compacting the data

## The CBOR format

The Compact Binary Object Representation (CBOR) [^3]is a format used to minimize the representation of structured data. It consists in a sequence of bytes fields, each starting with a one-byte marker for its type and argument, then a content depending upon the type.

[^3]: <https://datatracker.ietf.org/doc/html/rfc8949>

The type is encoded on 3 bits, thus coding 8 possible types. In the CLVR, only types 0 to 7 are used.

The argument is encoded on 5 bits, thus coding 64 possible arguments. Arguments below 0x18 are self-contained values, while arguments 0x18 to 0x1B means that the value is to be found in the next 1,2, 4 or 8 bytes. Further values are not used yet.

Table 2 - CBOR Types

| Type             | 3 bits | 5 bits      | Examples           | Decodes as                                                              |
|------------------|--------|-------------|--------------------|-------------------------------------------------------------------------|
| Unsigned integer | 0x00   | Value/Size  | 0x12<br>0x19 04 DD | 18 (0x12)<br>1245 (0x4DD)                                               |
| Negative integer | 0x20   | Value/Size  | 0x26<br>0x39 01 03 | -7 (-1-0x06)<br>-260 (-1-0x103)                                         |
| Byte string      | 0x40   | Length/Size | 0x58 40 …..        | Next 64 bytes                                                           |
| Text string      | 0x60   | Length/Size | 0x63 46 52 41      | FRA (UTF-8 46 52 41)                                                    |
| Array            | 0x80   | Length/Size | 0x84               | Array of 4 elements                                                     |
| Map              | 0xA0   | Length/Size | 0xA5               | Map of 5 items<br>Each item is a text string, followed by a CBOR field. |
| Tag              | 0xC0   | Tag id      | D2                 | Tag 18 (COSE object)                                                    |

Maps are associative arrays, that is a set of key-value pairs.

Tags are used to represent the role of a CBOR object, their values are published by the Internet Assigned Numbers Authority (IANA) at <https://www.iana.org/assignments/cbor-tags/cbor-tags.xhtml>

Usual tools and libraries for handling CBOR are converters between CBOR and the more frequent JSON format[^4].

[^4]: Such as <https://cbor.me/>

## Optimizing for CBOR

The CBOR representation is most efficient when handling numeric values. Text strings are just as large in CBOR as in the original format, plus a prefix for their type and length.

This implies that the use of text strings should be minimized:

-   Keys in maps (key-value pairs) should be short.
-   Dates should be represented as numbers rather than as text. “2025-03-14 07:37:49” requires 20 bytes to be represented in CBOR, while the equivalent Unix timestamp 1 754 379 469 is only 8 bytes long.
-   Vaccines are represented with their NUVA code as a number.

Unix timestamps are generally used in programming and are used for example in the standardized content header for representing the issuing date of a CLVR. But they count the number of elapsed seconds since January 1st, 1970. This is too precise for the need of vaccination history and does not go far enough in the past.

As an alternative, in the CLVR:

-   The date of birth is stored in textual format YYYY-MM-DD.
-   All vaccine administration dates are stored as a number of days since the date of birth.

Numeric values should also be as small as possible. To support this, it is suggested that the local index of the master record in its repository is unique only for a given day of administration. It allows to reinitialize it every day and always keep its value very small.

With these conventions, the JSON representation for the content of the example CLVR is as follows:
```json
{
"ver": "1.0.0",  
 "nam":{
"fnt": "DOË",
"gnt": "John"
},
"dob": "2017-07-19",
"v": [
{"reg": "FRA", "rep": 36, "i": 1245, "a": 1386, "mp": 29},
{"reg": "FRA", "rep": 36, "i": 127, "a": 1688, "mp": 644}
]
```
or, in CBOR representation (truncated to first vaccine):

<table>
<tbody>
<tr><td rowspan=24 style="vertical-align:top">Map(4)</td><td colspan=3></td><td>A4</td></tr>
    <tr><td colspan=3>"ver" (key)</td><td>53 76 65 72</td></tr>
    <tr><td colspan=3>"1.0.0" (value)</td><td>65 31 2E 30 2E 30</td></tr>
    <tr><td colspan=3>"nam" (key)</td><td>63 6E 61 6D</td></tr>
    <tr><td rowspan=5 style="vertical-align:top">"Map(2) (value)"</td><td colspan=2></td><td>A2</td></tr>
	    <tr><td colspan=2>"fnt" (key)</td><td>63 66 6E 74</td></tr>
	    <tr><td colspan=2>"DOË"</td><td>64 44 4F C3 8B</td></tr>
	    <tr><td colspan=2>"gnt" (key)</td><td>63 67 6E 74</td></tr>
	    <tr><td colspan=2>"John"</td><td>64 4A 6F 68 6E</td></tr>
    <tr><td colspan=3>"dob" (key)</td><td>63 64 6F 62</td></tr>
    <tr><td colspan=3>"2017-07-19" (value)</td><td>6A 32 30 31 37 2D 30 37 2D 31 39</td></tr>
    <tr><td colspan=3>"v" (key)</td><td>61 76</td></tr>
	<tr><td rowspan=12 style="vertical-align:top">Array[2] (value)</td><td colspan=2></td><td>82</td></tr>
	    <tr><td rowspan=11 style="vertical-align:top">Map(5) (element)</td><td></td><td>A5</td></tr>
		    <tr><td>"reg" (key)</td><td>63 72 65 67</td></tr>
		    <tr><td>"FRA" (value)</td><td>63 46 52 41</td></tr>
		    <tr><td>"rep" (key)</td><td>63 72 65 70</td></tr>
		    <tr><td>36 (value)</td><td>18 24</td></tr>
		    <tr><td>"i" (key)</td><td>61 69</td></tr>
		    <tr><td>1245 (value)</td><td>19 04 DD</td></tr>
		    <tr><td>"a" (key)</td><td>61 61</td></tr>
		    <tr><td>1386 (value)</td><td>19 05 6A</td></tr>
		    <tr><td>"mp" (key)</td><td>62 6D 70</td></tr>
		    <tr><td>29 (value)</td><td>18 1D</td></tr>
		   
</tbody>
</table>
