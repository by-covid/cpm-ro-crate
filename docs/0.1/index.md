# Common Provenance Model RO-Crate profile

* Title: Common Provenance Model RO-Crate profile
* Authors: Rudolf Wittner, Stian Soiland-Reyes, Simone Leo
* Date: 2023-01-11
* Version: 0.1
* Persistent identifier: <https://w3id.org/cpm/ro-crate/0.1>


Research objects, such as data, experimental results, computational models, or biological samples, are exchanged between organizations, so each of the organizations can provide provenance information only about a part of the research object’s life cycle. As a result, a complete provenance description of the object is then spread across different heterogeneous organizations. 

The [Common Provenance Model (CPM)](https://doi.org/10.1038/s41597-022-01537-6) provides a baseline for such distributed provenance chains. It defines how to interconnect distributed provenance parts encapsulated in PROV bundles, how to express standardized derivation paths between inputs and outputs of a process in a single bundle (so called provenance backbone), and how to attach domain specific information to the chain in a harmonized way. 

This document specifies how to identify and handle CPM compliant provenance files and CPM compliant meta-provenance files in an [RO-Crate](https://www.researchobject.org/ro-crate/). 


### General Requirements

* Each CPM compliant provenance bundle MUST be serialized into a standalone file. 
* The RO-Crate MAY contain multiple CPM compliant bundles/files. 
* The RO-Crate MUST include references to all CPM compliant provenance bundles/files present in the crate (arbitrary provenance files or log files do not need to be mentioned).
    * _Rationale: Each CPM provenance bundle is part of a distributed provenance chain. As a consequence, any such bundle can be referenced from other parts of the chain, which can be stored externally (outside the crate). For that reason, the RO-Crate must provide means to identify and locate any of the CPM compliant provenance bundles present in the crate._
* The RO-Crate MAY include a meta provenance file. Multiple meta-provenance bundles MAY be present in the meta provenance file. 
    * _Rationale: This is to keep meta provenance handling simple. If multiple meta provenance files would be allowed, then we would have to set requirements on how meta provenance can be split across the files, which might introduce unnecessary complexity._
* The RO Crate MUST include a reference to the meta provenance file, if present. 

 <table>
    <tr>
      <th>Type/Property</th>
      <th>Required?</th>
      <th>Description</th>
    </tr>
    <tr>
      <td colspan="3"></td>
    </tr>
    <tr id="CPMProvenanceFile">
      <th colspan="3"><strong>CPMProvenanceFile</strong><br />
      extends <a href="http://schema.org/MediaObject">MediaObject</a> (@id is
      resolvable), dataEntity</th>
    </tr>
    <tr>
      <td>@type</td>
      <td>MUST</td>
      <td>
        Type that identifies the CPM provenance file.
        <p>Array MUST include "File". Array MUST include "CPMProvenanceFile".</p>
      </td>
    </tr>
    <tr>
      <td>@id</td>
      <td>MUST</td>
      <td>
        Identifier of the CPM provenance file.
        <p>SHOULD be a relative URI to a data entity in the crate (e.g.
        <code>"provenance/prov-training.provn"</code>) but MAY be an absolute URI .
        Resolving this identifier MUST return this provenance file in the given
        format.</p>
      </td>
    </tr>
    <tr>
      <td><a href="https://schema.org/identifier">identifier</a></td>
      <td>SHOULD</td>
      <td>
        Identifier of a provenance bundle present in the CPM provenance file.
        <p>MUST be an absolute URI. MUST match the expanded bundle identifier. MAY be
        equal to @id if absolute.</p>
        <p>Note: PROV formats that support identified bundles SHOULD ensure their internally defined identifier also matches this identifier.</p>
      </td>
    </tr>
    <tr>
      <td><a href="http://schema.org/dateModified">dateModified</a></td>
      <td>SHOULD</td>
      <td>
        The time this CPM provenance file was last modified/written (not necessarily when
        the bundle included was finalized or the file was added to the RO-Crate).
        <p>MUST be a string with format "ddMMYYYY".</p>
      </td>
    </tr>
    <tr>
      <td><a href="http://schema.org/encodingFormat">encodingFormat</a></td>
      <td>MUST</td>
      <td>
        Encoding of the CPM provenance file.
        <p>Array MUST contain a string indicating the IANA media type of the file, e.g.
        <code>text/turtle</code> or <code>text/provenance-notation</code> or <code>application/ld+json</code>.</p>
        <p>Array MUST also contain a reference to a CreativeWork that indicates the PROV
        format used in the serialization, which <code>@id</code> SHOULD be one of:</p>
        <ul>
          <li><a href=
          "http://www.w3.org/TR/2013/REC-prov-n-20130430/">http://www.w3.org/TR/2013/REC-prov-n-20130430/</a>
          (PROV-N)</li>
          <li><a href=
          "http://www.w3.org/TR/2013/REC-prov-o-20130430/">http://www.w3.org/TR/2013/REC-prov-o-20130430/</a>
          (PROV-O as RDF or OWL)</li>
          <li><a href=
          "http://www.w3.org/TR/2013/NOTE-prov-xml-20130430/">http://www.w3.org/TR/2013/NOTE-prov-xml-20130430/</a>
          (PROV-XML)</li>
          <li>
            <a href=
            "http://www.w3.org/Submission/2013/SUBM-prov-json-20130424/">http://www.w3.org/Submission/2013/SUBM-prov-json-20130424/</a>
            (PROV-JSON)
            <p>Example:</p>
            <pre>
{ "@id: "provone.ttl", 
  "@type" ["File", "CPMProvenanceFile"],
  "encodingFormat": [ 
      "text/turtle", 
      {"@id": "http://www.w3.org/TR/2013/REC-prov-o-20130430/"}
   ]
},
{ "@id": "http://www.w3.org/TR/2013/REC-prov-o-20130430/", 
  "@type": "CreativeWork",
  "name": "PROV-O format"
}              
</pre>
          </li>
        </ul>
      </td>
    </tr>
    <tr>
      <td><a href="http://schema.org/about">about</a></td>
      <td>SHOULD</td>
      <td>
        Array contains entity identifiers, which are documented by the CPM provenance
        file.
        <p>SHOULD contain at least one identifier.</p>
      </td>
    </tr>
    <tr id="CPMMetaProvenanceFile">
      <th colspan="3"><strong>CPMMetaProvenanceFile</strong><br />
      extends CPMProvenanceFile</th>
    </tr>
    <tr>
      <td>@type</td>
      <td>MUST</td>
      <td>
        Type that identifies the CPM meta provenance file.
        <p>Array MUST include <code>File</code> and <code>CPMMetaProvenanceFile</code></p>
      </td>
    </tr>
    <tr>
      <td>@id</td>
      <td>MUST</td>
      <td>
        Identifier of the CPM meta provenance file.
        <p>SHOULD be an absolute URI, but MAY be a relative URI to a data entity in the
        crate (e.g. <code>"provenance/prov-meta.jsonld"</code>)</p>
      </td>
    </tr>
    <tr>
      <td><a href="http://schema.org/dateModified">dateModified</a></td>
      <td>SHOULD</td>
      <td>
        The time this CPM meta provenance file was last modified/written (not necessarily
        when the bundle included was finalized or the file was added to the RO-Crate).
        <p>MUST be a string with format "ddMMYYYY".</p>
      </td>
    </tr>
    <tr>
      <td><a href="http://schema.org/encodingFormat">encodingFormat</a></td>
      <td>MUST</td>
      <td>
        Encoding of the CPM meta provenance file.
        <p>Array MUST contain a string indicating the IANA media type of the file, e.g.
        <code>text/turtle</code> or <code>text/provenance-notation</code> or <code>application/ld+json</code>.</p>
        <p>Array MUST also contain a reference to a CreativeWork that indicates the PROV
        format used in the serialization, which <code>@id</code> SHOULD be one of:</p>
        <ul>
          <li><a href=
          "http://www.w3.org/TR/2013/REC-prov-n-20130430/">http://www.w3.org/TR/2013/REC-prov-n-20130430/</a>
          (PROV-N)</li>
          <li><a href=
          "http://www.w3.org/TR/2013/REC-prov-o-20130430/">http://www.w3.org/TR/2013/REC-prov-o-20130430/</a>
          (PROV-O as RDF or OWL)</li>
          <li><a href=
          "http://www.w3.org/TR/2013/NOTE-prov-xml-20130430/">http://www.w3.org/TR/2013/NOTE-prov-xml-20130430/</a>
          (PROV-XML)</li>
          <li>
            <a href=
            "http://www.w3.org/Submission/2013/SUBM-prov-json-20130424/">http://www.w3.org/Submission/2013/SUBM-prov-json-20130424/</a>
            (PROV-JSON) Example:
            <pre>
{ "@id: "provone.ttl", 
   "@type" ["File", "CPMProvenanceFile"],
   "encodingFormat": [
      "text/turtle",
      {"@id": "http://www.w3.org/TR/2013/REC-prov-o-20130430/"}
   ]
},
{  "@id": "http://www.w3.org/TR/2013/REC-prov-o-20130430/",
   "@type": "CreativeWork",
   "name": "PROV-O format"
}             
</pre>
          </li>
        </ul>
      </td>
    </tr>
    <tr>
      <td><a href="http://schema.org/hasPart">hasPart</a></td>
      <td>MUST</td>
      <td>
        Identifiers of meta provenance bundles present in the CPM meta provenance file.
        <p>Array MUST contain absolute URIs. URIs MUST match the expanded bundle
        identifiers as used internally in the CPM provenance files.</p>
      </td>
    </tr>
</table>


## Example

Example of the CPM RO-Crate profile usage is presented in Zenodo repository, available on this link: <https://doi.org/10.5281/zenodo.7676924>

## Notes

* Some PROV formats like PROV-N and PROV-O in JSON-LD support multiple bundles in the same document. This feature can be used if there is no need for different access control on different bundles.

* PROV formats that support identified bundles SHOULD ensure their internally defined identifier also match this identifier.     
