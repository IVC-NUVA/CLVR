# CLVR Content

## CLVR logical content

Given the above requirements, the CLVR must then contain:

-   Minimal patient identity traits: name, given name and date of birth.
-   For each administered vaccine:
    -   used vaccine product,
    -   date of administration,
    -   reference to the master record, identified by a national registry, a repository of master records within this registry and a local index in this repository.
-   A digital signature of the above contents

## Example data

In this document, the CLVR content will be exemplified using the data found in the EVC generator demonstrator[^2].

[^2]: <https://github.com/EUVABECO/EVC-generator>

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

-   A signature characterization header (5.2)
-   A content header (see 4.3)
-   The actual content, consisting of the patient identity traits and the administered vaccines (4.2)
-   The signature value (5.3)

The diagram below presents this structure with the typical size in bytes of the different sections (before compression).
<table>
<thead style="font-weight:bold">
<tr><td></td><td></td><td>Identity</td><td colspan=8>Vaccines</td><td>Signature</td></tr>
</thead>
<tbody>
<tr><td>30</td><td>24</td><td>110</td><td>31</td><td>31</td><td>31</td><td>31</td><td>31</td><td>31</td><td>31</td><td>31</td><td>56</td></tr>
</tbody>
</table>


