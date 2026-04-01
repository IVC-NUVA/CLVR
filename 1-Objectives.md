# CLVR objectives

The CLVR is a digital content that:

-   Represents all the vaccines received by a person in its lifetime.
-   Is usable to determine the next required administrations
-   Can be encoded within a single QR Code.
-   Can be verified for authenticity.
-   Includes references to reliable sources for each recorded vaccination.

## Lifetime vaccination history

When considering all vaccination schedules, even for persons with special or increased vaccination needs, the total number of vaccines received over a lifetime remains below 65.

This figure is obtained with the convention that for yearly vaccines, like influenza or now COVID-19, only the 5 last administrations are recorded.

To ensure robustness and longevity of the standard, we adopt significant design margins through all this document. The discussions below will use a target of holding **100 vaccine administration events** within a CLVR.

## Useability

Contrary to most medications, vaccines are not substitutable even if they have a same functional role. It is then necessary to record the vaccines at the best possible precision level, down to the brand name if it is available from the data source. This is why functional encodings such as ATC or SNOMED-CT are not sufficient, and a the NUVA code system, designed for vaccination decision support, is used.

## Compacity

As discussed in the [QR code representation](7-QRCode.md) page , the whole CLVR should fit in less than 1800 bytes in binary format, that is 2700 alphanumeric characters in Base45 format.

## Authenticity

The CLVR should be verifiable by the receiving health professional, whatever its origin, with:

-   A minimum set of identity traits for the presenting patient. The purpose is not to make the CLVR an identity document, but only to have a minimal set of information to be compared with an approved identity document.
-   A digital signature to detect any tampering with the CLVR contents.

## References to reliable sources

When a health professional administers a vaccine and issues a new CLVR, prior vaccinations are included based on the information from the previous CLVR. Each administration event is also recorded in a repository system which serves as the authoritative source of record. These repositories  may be national or distributed across health facilities, and are referenced within a national registry or repositories.