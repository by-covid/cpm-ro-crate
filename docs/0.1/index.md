<!-- Output copied to clipboard! -->

<!-- Yay, no errors, warnings, or alerts! -->


# Common Provenance Model RO-Crate profile

Research objects, such as data, experimental results, computational models, or biological samples, are exchanged between organizations, so each of the organizations can provide provenance information only about a part of the research object’s life cycle. As a result, a complete provenance description of the object is then spread across different heterogeneous organizations. The Common Provenance Model (CPM) provides a baseline for such distributed provenance chains. It defines how to interconnect distributed provenance parts encapsulated in PROV bundles, how to express standardized derivation paths between inputs and outputs of a process in a single bundle (so called provenance backbone), and how to attach domain specific information to the chain in a harmonized way. 

This document specifies how to identify and handle CPM compliant provenance files and CPM compliant meta-provenance files in an RO-Crate. 

CPM Publication: [https://doi.org/10.1038/s41597-022-01537-6](https://doi.org/10.1038/s41597-022-01537-6) 


### **General Requirements**



* Each CPM compliant provenance bundle MUST be serialized into a standalone file. 
* The RO-Crate MAY contain multiple CPM compliant bundles/files. 
* The RO-Crate MUST include references to all CPM compliant provenance bundles/files present in the crate (arbitrary provenance files or log files do not need to be mentioned).
    * _Rationale: Each CPM provenance bundle is part of a distributed provenance chain. As a consequence, any such bundle can be referenced from other parts of the chain, which can be stored externally (outside the crate). For that reason, the RO-Crate must provide means to identify and locate any of the CPM compliant provenance bundles present in the crate. _
* The RO-Crate MAY include a meta provenance file. Multiple meta-provenance bundles MAY be present in the meta provenance file. 
    * _Rationale: This is to keep meta provenance handling simple. If multiple meta provenance files would be allowed, then we would have to set requirements on how meta provenance can be split across the files, which might introduce unnecessary complexity._
* The RO Crate MUST include a reference to the meta provenance file, if present. 

<table>
  <tr>
   <td>
<strong>Type/Property</strong>[^1]
   </td>
   <td><strong>Required?</strong>
   </td>
   <td><strong>Description</strong>
   </td>
  </tr>
  <tr>
   <td colspan="3" ><strong>CPMProvenanceFile \
</strong>extends <a href="http://schema.org/MediaObject">MediaObject</a> (@id is resolvable), dataEntity 
   </td>
  </tr>
  <tr>
   <td>@type
   </td>
   <td>MUST
   </td>
   <td>Type that identifies the CPM provenance file.
<p>
Array MUST include "File". Array MUST include  “CPMProvenanceFile”.
   </td>
  </tr>
  <tr>
   <td>@id
   </td>
   <td>MUST
   </td>
   <td>Identifier of the CPM provenance file.
<p>
SHOULD be a relative URI to a data entity in the crate (e.g. <code>"provenance/prov-training.provn"</code>) but MAY be an absolute URI . Resolving this identifier MUST return this provenance file in the given format.
   </td>
  </tr>
  <tr>
   <td><a href="https://schema.org/identifier">identifier</a>
   </td>
   <td>SHOULD
   </td>
   <td>Identifier of a provenance bundle present in the CPM provenance file. 
<p>
MUST be an absolute URI. MUST match the expanded bundle identifier[^2]. MAY be equal to @id if absolute. 
   </td>
  </tr>
  <tr>
   <td><a href="http://schema.org/dateModified">dateModified</a>
   </td>
   <td>SHOULD
   </td>
   <td>The time this CPM provenance file was last modified/written (not necessarily when the bundle included was finalized or the file was added to the RO-Crate).
<p>
MUST be a string with format “ddMMYYYY”.
   </td>
  </tr>
  <tr>
   <td><a href="http://schema.org/encodingFormat">encodingFormat</a>
   </td>
   <td>MUST
   </td>
   <td>Encoding of the CPM provenance file.
<p>
Array MUST contain a string indicating the IANA media type of the file, e.g. "text/turtle" or "text/provenance-notation" or "application/ld+json".
<p>
Array MUST also contain a reference to a CreativeWork that indicates the PROV format used in the serialization. "@id" SHOULD be one of:
<ul>

<li><a href="http://www.w3.org/TR/2013/REC-prov-n-20130430/">http://www.w3.org/TR/2013/REC-prov-n-20130430/</a> (PROV-N)

<li><a href="http://www.w3.org/TR/2013/REC-prov-o-20130430/">http://www.w3.org/TR/2013/REC-prov-o-20130430/</a> (PROV-O as RDF or OWL)

<li><a href="http://www.w3.org/TR/2013/NOTE-prov-xml-20130430/">http://www.w3.org/TR/2013/NOTE-prov-xml-20130430/</a> (PROV-XML)

<li><a href="http://www.w3.org/Submission/2013/SUBM-prov-json-20130424/">http://www.w3.org/Submission/2013/SUBM-prov-json-20130424/</a> (PROV-JSON)

<p>
Example:
<p>
{ “@id: “provone.ttl”, “@type” [“File”, ”CPMProvenanceFile”],
<p>
“encodingFormat”: [ \
  “text/turtle”, \
  {“@id”: “http://www.w3.org/TR/2013/REC-prov-o-20130430/”} \
]
<p>
{“@id”: “http://www.w3.org/TR/2013/REC-prov-o-20130430/”, \
“@type”: “CreativeWork”}
</li>
</ul>
   </td>
  </tr>
  <tr>
   <td><a href="http://schema.org/about">about</a>
   </td>
   <td>SHOULD
   </td>
   <td>Array contains entity identifiers, which are documented by the CPM provenance file. 
<p>
SHOULD contain at least one identifier. 
   </td>
  </tr>
  <tr>
   <td colspan="3" ><strong>CPMMetaProvenanceFile \
</strong>extends CPMProvenanceFile
   </td>
  </tr>
  <tr>
   <td>@type
   </td>
   <td>MUST
   </td>
   <td>Type that identifies the CPM meta provenance file.
<p>
Array MUST include "File". MUST include CPMMetaProvenanceFile
   </td>
  </tr>
  <tr>
   <td>@id
   </td>
   <td>MUST
   </td>
   <td>Identifier of the CPM meta provenance file.
<p>
SHOULD be an absolute URI , but MAY be a relative URI to a data entity in the crate (e.g. <code>"provenance/prov-meta.jsonld"</code>) 
   </td>
  </tr>
  <tr>
   <td><a href="http://schema.org/dateModified">dateModified</a>
   </td>
   <td>SHOULD
   </td>
   <td>The time this CPM meta provenance file was last modified/written (not necessarily when the bundle included was finalized or the file was added to the RO-Crate).
<p>
MUST be a string with format “ddMMYYYY”.
   </td>
  </tr>
  <tr>
   <td><a href="http://schema.org/encodingFormat">encodingFormat</a>
   </td>
   <td>MUST
   </td>
   <td>Encoding of the CPM meta provenance file.
<p>
Array MUST contain a string indicating the IANA media type of the file, e.g. "text/turtle" or "text/provenance-notation" or "application/ld+json".
<p>
Array MUST also contain a reference to a CreativeWork that indicates the PROV format used in the serialization. "@id" SHOULD be one of:
<ul>

<li><a href="http://www.w3.org/TR/2013/REC-prov-n-20130430/">http://www.w3.org/TR/2013/REC-prov-n-20130430/</a> (PROV-N)

<li><a href="http://www.w3.org/TR/2013/REC-prov-o-20130430/">http://www.w3.org/TR/2013/REC-prov-o-20130430/</a> (PROV-O as RDF or OWL)

<li><a href="http://www.w3.org/TR/2013/NOTE-prov-xml-20130430/">http://www.w3.org/TR/2013/NOTE-prov-xml-20130430/</a> (PROV-XML)

<li><a href="http://www.w3.org/Submission/2013/SUBM-prov-json-20130424/">http://www.w3.org/Submission/2013/SUBM-prov-json-20130424/</a> (PROV-JSON)

<p>
Example:
<p>
{ “@id: “provone.ttl”, “@type” [“File”, ”CPMProvenanceFile”],
<p>
“encodingFormat”: [ \
  “text/turtle”, \
  {“@id”: “http://www.w3.org/TR/2013/REC-prov-o-20130430/”} \
]
<p>
{“@id”: “http://www.w3.org/TR/2013/REC-prov-o-20130430/”, \
“@type”: “CreativeWork”}
</li>
</ul>
   </td>
  </tr>
  <tr>
   <td><a href="http://schema.org/hasPart">hasPart</a>
   </td>
   <td>MUST
   </td>
   <td>Identifiers of meta provenance bundles present in the CPM meta provenance file. 
<p>
Array MUST contain absolute URIs. URIs MUST match the expanded bundle identifiers as used internally in the CPM provenance files. 
   </td>
  </tr>
</table>



## Example

The example RO-Crate documents a single step computation implemented as a python script which takes a file as input and generates another file as output. In addition, the computation generated an arbitrary log file, which was transformed into a CPM compliant provenance bundle using a standalone python script. The resulting CPM provenance file documents the computation execution. 


```
{ "@context": ["https://w3id.org/ro/crate/1.1/context",
  { "CPMProvenanceFile": "https://w3id.org/ro/terms/cpm#CPMProvenanceFile",
    "CPMMetaProvenanceFile": "https://w3id.org/ro/terms/cpm#CPMMetaProvenanceFile",
  ],
  "@graph": [

 {
    "@type": "CreativeWork",
    "@id": "ro-crate-metadata.json",
    "conformsTo": {"@id": "https://w3id.org/ro/crate/1.1"},
    "about": {"@id": "./"}
 },
 {
    "@id": "./",
    "@type": "Dataset",
    "datePublished": "2022",
    "conformsTo": [
       {"@id": "https://w3id.org/ro/wfrun/0.1/process"},
	{"@id": "https://w3id.org/cpm/crate/0.1??"},
    ],
    "name": "",
    "description": "",
    "hasPart": [
        {
        "@id": "INPUT_DATASET_PATH"
        },
        {
          "@id": "OUTPUT_DATASET_PATH"
        },
        {
          "@id": "COMPUTATION_LOG_FILE"
        },
        {
          "@id": "CPM_COMPLIANT_PROVENANCE"
        },
        {
          "@id": "COMPUTATION_SCRIPT"
        },
        {
          "@id": "CPM_PROVENANCE_GENERATION_SCRIPT"
        }
      ],
	"mentions": [
		{ "@id": "#Exec-computation"},
		{ "@id": "#Exec-CPM-provgen"}
	]
 },
 {
   "@id": "INPUT_DATASET_PATH",
   "@type": "File",
   "description": "",
   "name": ""
 },
 {
   "@id": "OUTPUT_DATASET_PATH",
   "@type": "File",
   "description": "",
   "name": ""
 },
 {
   "@id": "COMPUTATION_SCRIPT",
   "@type": ["File","SoftwareSourceCode"],
   "description": "",
   "name": ""
 },
 {
   "@id": "COMPUTATION_LOG_FILE",
   "@type": "File",
   "description": "A log file generated by the computation
 execution.",
   "name": ""
 },
 {
   "@id": "CPM_PROVENANCE_GENERATION_SCRIPT",
   "@type": ["File","SoftwareSourceCode"],
   "description": "A python script that translates the computation log
 files into CPM compliant provenance file.",
   "name": ""
 },
 {
   "@id": "CPM_COMPLIANT_PROVENANCE",
   "@type": ["File", "CPMProvenanceFile"],
   "description": "CPM compliant provenance file generated based on 
the computation log file.",
   "encodingFormat": [
	"text/provenance-notation",
{ "@id": "http://www.w3.org/TR/2013/REC-prov-n-20130430/"},
   "name": "",
   "about": [{"@id": "#Exec-computation"}]

 },
 {
   "@id": "#Exec-computation",
   "@type": ["CreateAction"],
   "description": "Computation execution.",
   "name": "",
    "instrument": {
          "@id": "COMPUTATION_SCRIPT"
    },
    "object": [
          {"@id": "INPUT_DATASET_PATH"}
    ],
    "result": [
          {"@id": "OUTPUT_DATASET_PATH"},
          {"@id": "COMPUTATION_LOG_FILE"}
        ]
 },
 {
   "@id": "#Exec-CPM-provgen",
   "@type": ["CreateAction"],
   "description": "CPM compliant provenance generation.",
   "name": "",
   "instrument": {
        "@id": "CPM_PROVENANCE_GENERATION_SCRIPT"
      },
    "object": [
        {"@id": "COMPUTATION_LOG_FILE"}
    ],
    "result": [
          {"@id": "CPM_COMPLIANT_PROVENANCE"}
        ]
 }
 ]
}
```



## Obsolete notes

For the implementation of case 1) (provenance is provided to consumers in an original storage format), we have chosen RO-Crates[^3]. RO Crate is … In our implementation, we take an existing AI based computational workflow implemented as a set of python scripts, express the computational results using RO-Crate, generate provenance information in accordance with the specification of RO Crate and the extension of the CPM. 

According to the common provenance model, the resulting provenance must contain a meta-bundle. In this case, the meta bundle documents the history of three _root bundles_, each of them describing a standalone part of the AI workflow - data preprocessing, model training, model testing. 

[Link to the RO Crate](https://drive.google.com/drive/folders/1VO82z0YKX6lVucJXfDXe3ldF3JOXB34S?usp=sharing).

Since RO Crates primarily work on a file system level, the most natural way to express a bundle in an RO Crate is to serialize each bundle into a separate file[^4]. This allows a natural mapping between the RO Crate’s data item properties and bundles. 

As a result, each RO-Crate must contain:



1. Do we also want to include the identifier of a bundle in the RO crate? This might be necessary if we allow multiple bundles in a single file, to enable references to a specific bundle. This specification also does not cover cases when (for instance) we have provn with 2 named bundles and one top-level-instance bundle. 


### **General Requirements**


<table>
  <tr>
   <td><strong>Type/Property</strong>[^5]
   </td>
   <td><strong>Required?</strong>
   </td>
   <td><strong>Description</strong>
   </td>
  </tr>
  <tr>
   <td colspan="3" ><strong>CPMProvenanceFile \
</strong>extends <a href="http://schema.org/MediaObject">MediaObject</a> (@id is resolvable), dataEntity 
   </td>
  </tr>
  <tr>
   <td>@type
   </td>
   <td>MUST
   </td>
   <td>Array MUST include "File". MUST include  CPMProvenanceFile or its subtype CPMMetaProvenanceFile
   </td>
  </tr>
  <tr>
   <td>@id
   </td>
   <td>MUST
   </td>
   <td>SHOULD be an absolute URI but MAY be a relative URI to a data entity in the crate (e.g. <code>"provenance/prov-training.provn"</code>). Resolving this identifier MUST return this provenance file in the given format.
   </td>
  </tr>
  <tr>
   <td><a href="https://schema.org/identifier">identifier</a>
   </td>
   <td>SHOULD
   </td>
   <td>If this provenance file contains only a single fragment or meta bundle, then this property MUST be present.  \
 \
If present: \
MUST be absolute URI. MUST match expanded bundle identifier as defined within the meta-bundle provenance[^6]. MAY be equal to @id if absolute. 
<p>
If not present: property hasPart must be specified.
   </td>
  </tr>
  <tr>
   <td><a href="https://schema.org/hasPart">hasPart</a> 
   </td>
   <td>SHOULD
   </td>
   <td>If this provenance file contains more than one fragment bundle, then this property MUST be present to identify them. 
<p>
If present, each of the members: \
MUST be absolute URI. MUST match expanded bundle identifier as defined within the meta-bundle provenance. 
   </td>
  </tr>
  <tr>
   <td>fileHash
   </td>
   <td>SHOULD
   </td>
   <td>0 or more PropertyValue (placeholder for file content checksum, to be decided). Producers who are updating the provenance file MUST update or remove this property.
   </td>
  </tr>
  <tr>
   <td>dateModified
   </td>
   <td>SHOULD
   </td>
   <td>The time this provenance file was last modified/written (not necessarily when its bundles were finalized or the file was added to the RO-Crate)
   </td>
  </tr>
  <tr>
   <td><a href="http://schema.org/encodingFormat">encodingFormat</a>
   </td>
   <td>MUST
   </td>
   <td>Array MUST contain a string indicate the IANA media type of the file, e.g. "text/turtle" or "text/provenance-notation" or "application/ld+json".
<p>
Array MUST also contain a reference to CreativeWork that indicates the PROV format used in the serialization. "@id" SHOULD be one of:
<ul>

<li><a href="http://www.w3.org/TR/2013/REC-prov-n-20130430/">http://www.w3.org/TR/2013/REC-prov-n-20130430/</a> (PROV-N)

<li><a href="http://www.w3.org/TR/2013/REC-prov-o-20130430/">http://www.w3.org/TR/2013/REC-prov-o-20130430/</a> (PROV-O as RDF or OWL)

<li><a href="http://www.w3.org/TR/2013/NOTE-prov-xml-20130430/">http://www.w3.org/TR/2013/NOTE-prov-xml-20130430/</a> (PROV-XML)

<li><a href="http://www.w3.org/Submission/2013/SUBM-prov-json-20130424/">http://www.w3.org/Submission/2013/SUBM-prov-json-20130424/</a> (PROV-JSON)

<p>
Example:
<p>
{ “@id: “provone.ttl”, “@type” cpm:CPMProvenanceFile,
<p>
“encodingFormat”: [ \
  “text/turtle”, \
  {“@id”: “http://www.w3.org/TR/2013/REC-prov-o-20130430/”} \
]
<p>
{“@id”: “http://www.w3.org/TR/2013/REC-prov-o-20130430/”, \
“@type”: “CreativeWork”}
</li>
</ul>
   </td>
  </tr>
  <tr>
   <td>conformsTo
   </td>
   <td>SHOULD
   </td>
   <td>If this bundle has domain-specific provenance, conformsTo SHOULD point to a CreativeWork that indicates the profile or domain-specific vocabulary used.
   </td>
  </tr>
  <tr>
   <td>…
   </td>
   <td>
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td colspan="3" ><strong>CPMMetaProvenanceFile \
</strong>extends CPMProvenanceFile
   </td>
  </tr>
  <tr>
   <td>@type
   </td>
   <td>MUST
   </td>
   <td>Array MUST include "File". MUST include CPMMetaProvenanceFile
   </td>
  </tr>
  <tr>
   <td>@id
   </td>
   <td>MUST
   </td>
   <td>SHOULD be an absolute URI , but MAY be a relative URI to a data entity in the crate (e.g. <code>"provenance/prov-meta.jsonld"</code>) 
   </td>
  </tr>
  <tr>
   <td>
   </td>
   <td>
   </td>
   <td>(same properties as for <strong>CPMProvenanceFile</strong>)
   </td>
  </tr>
  <tr>
   <td>identifier
   </td>
   <td>SHOULD
   </td>
   <td>Be absolute URI that identify the <em>meta bundle</em> that is contained by this provenance file. 
   </td>
  </tr>
  <tr>
   <td>hasPart
   </td>
   <td>SHOULD NOT
   </td>
   <td>Meta provenance files SHOULD NOT include other provenance bundles.
   </td>
  </tr>
  <tr>
   <td>about
   </td>
   <td>MUST
   </td>
   <td>Array MUST list absolute URI of every <em>root bundle</em> which collection are defined within the contained meta bundle(s). 
   </td>
  </tr>
</table>



<!-- Footnotes themselves at the bottom. -->
## Notes

[^1]:
<p>
     The table structure copied from ro crate workflow run draft

[^2]:
<p>
     PROV formats that support identified bundles SHOULD ensure their internally defined identifier also matches this identifier.

[^3]:
     [https://www.researchobject.org/ro-crate/](https://www.researchobject.org/ro-crate/) 

[^4]:
     Some PROV formats like PROV-N and PROV-O in JSON-LD support multiple bundles in the same document. This feature can be used if there is no need for different access control on different bundles.

[^5]:
<p>
     The table structure copied from ro crate workflow run draft

[^6]:
<p>
     PROV formats that support identified bundles SHOULD ensure their internally defined identifier also match this identifier.
