
# Common Provenance Model RO-Crate profile

Research objects, such as data, experimental results, computational models, or biological samples, are exchanged between organizations, so each of the organizations can provide provenance information only about a part of the research object’s life cycle. As a result, a complete provenance description of the object is then spread across different heterogeneous organizations. 

The [Common Provenance Model (CPM)](https://doi.org/10.1038/s41597-022-01537-6) provides a baseline for such distributed provenance chains. It defines how to interconnect distributed provenance parts encapsulated in PROV bundles, how to express standardized derivation paths between inputs and outputs of a process in a single bundle (so called provenance backbone), and how to attach domain specific information to the chain in a harmonized way. 

This document specifies how to identify and handle CPM compliant provenance files and CPM compliant meta-provenance files in an [RO-Crate](https://www.researchobject.org/ro-crate/). 

## Releases

* [cpm-ro-crate 0.1](0.1/) (PID: <https://w3id.org/cpm/ro-crate/0.1>)
* [cpm-ro-crate 0.2](0.2/) (PID: <https://w3id.org/cpm/ro-crate/0.2>)

## Vocabulary

The following terms are defined in the namespace <https://w3id.org/cpm/ro-crate#> and can be used with RO-Crate when conforming to this profile.

### CPMProvenanceFile

* Permalink: <https://w3id.org/cpm/ro-crate#CPMProvenanceFile>
* Definition: A provenance file in a PROV format, that follows the expectations of the [Common Provenance Model (CPM)](https://doi.org/10.1038/s41597-022-01537-6)
* Expected properties: [CPMProvenanceFile requirements](0.1/#CPMProvenanceFile)

### CPMMetaProvenanceFile

* Permalink: <https://w3id.org/cpm/ro-crate#CPMMetaProvenanceFile>
* Subclass of: `CPMProvenanceFile`
* Definition: A meta-provenance file that contains provenance of another provenance file.
* Expected properties: [CPMProvenanceFile requirements](0.1/#CPMMetaProvenanceFile)


