.. _cellmlmetaspec-citation:

==========================================
CellML Citation Metadata Specification 2.0
==========================================

Last Updated: October 2011

:Authors:
   Michael T. Cooling (m.cooling@auckland.ac.nz)

**Status of this Document**

Discussion draft.

**Dependencies**

This specification is dependent on the CellML Core Metadata Specification 2.0.

Introduction
============

This document describes how to add annotation linking the CellML model document, or an element of that document, to one or more citable works such as journal articles or conference presentations.

Realisation Strategy
====================

Citable works are linked to a model document or an element of that document by making the element an RDF Subject, and the citable work the RDF Object, of an RDF statement.  The Predicate for this relationship should be the Biomodels Qualifier (http://biomodels.net/qualifiers/) 'description'.

Where multiple citable works are required, multiple RDF statements could be made. Alternatively, multiple citable works could be specified using the RDF 'Bag' container. See http://www.w3.org/TR/rdf-primer/#containers for more information on RDF containers.

Citable works (the RDF Objects) should be specified as URIs wherever possible. If the work has a record in a persistent database such as Pubmed (http://www.ncbi.nlm.nih.gov/pubmed/), then the recommended method is to use an Identifiers.org URI (see http://identifiers.org/ for more on Identifiers.org).  Where this is not applicable (if, for example, the work does not have a record, or perhaps is not published at all) another URI may be used (in some examples below, ISSN URNs are used).  Care should be taken that the URI be as discoverable and long-lived as possible in order for someone to gain meaningful information from it.

If an appropriate URI is not available, an alternative method is to provide a BIBO (http://bibliontology.com/specification) Object in the RDF to represent the bibliographic resource. This object can then be linked to the model element of interest with 'description'. Any BIBO object may be encoded, and it is recommended that as many BIBO properties should be added as necessary so that someone can clearly identify the cited work. At the time of writing, the recommended BIBO version is the latest one, labelled 'Revision 1.3'. A browsable list of BIBO classes and properties can be found at  http://bibotools.googlecode.com/svn/bibo-ontology/trunk/doc/index.html.

The recommended BIBO and Biomodels qualifer namespace declarations are as follows:

.. code-block:: xml

   xmlns:bibo="http://purl.org/ontology/bibo/"
   xmlns:bqmodel="http://biomodels.net/model-qualifiers"

Several of the BIBO properties are 'borrowed' from other collections of properties such as the Dublin Core, and FOAF which are also used elsewhere in the CellML Metadata Framework 2.0 (see the CellML Basic Information Metadata Specification 2.0 for details), hence using those properties in a BIBO object may also require the specification in a CellML Model Document of additional namespaces, if they are not already defined. Several cases of this are shown in the Examples below.
Examples

**1. A model is described in a published journal article that is identified in Pubmed.**

.. code-block:: xml

   <?xml version="1.0"?>

   <model xmlns="http://www.cellml.org/cellml/1.1#"
   xmlns:cmeta="http://www.cellml.org/metadata/2.0#"
   cmeta:id="ip3_model"
   name="ip3_model"
   xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
   xmlns:bqmodel="http://biomodels.net/model-qualifiers/"
   xmlns:bibo="http://purl.org/ontology/bibo/"
   >

   <rdf:RDF>
       <rdf:Description rdf:about="#ip3_model">
           <bqmodel:description rdf:resource="
           http://identifiers.org/pubmed/17693463"/>
       </rdf:Description>
   </rdf:RDF>

   ...other elements...

   </model>

**2. A model is described in a published journal article which lacks an identifier in Pubmed.**

.. code-block:: xml

   <model xmlns="http://www.cellml.org/cellml/1.1#"
   xmlns:cmeta="http://www.cellml.org/metadata/2.0#"
   cmeta:id="ip3_model"
   name="ip3_model"
   xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
   xmlns:bqmodel="http://biomodels.net/model-qualifiers/"
   xmlns:bibo="http://purl.org/ontology/bibo/"
   xmlns:dcterms="http://purl.org/dc/terms/"
   >

   <rdf:RDF>
       <rdf:Description rdf:about="#ip3_model">
           <bqmodel:description rdf:resource="#example_article"/>
       </rdf:Description>

       <bibo:Article rdf:ID="example_article">
           <dcterms:creator>Fred Bagg</dcterms:creator>
           <dcterms:issued>1981</dcterms:issued>
       <dcterms:title>Pertubations in calcium signaling activate immune system function</dcterms:title>
           <bibo:volume>66</bibo:volume>
           <bibo:issue>10</bibo:issue>
           <bibo:pageStart>1102</bibo:pageStart>
           <bibo:pageEnd>1111</bibo:pageEnd>
           <dcterms:isPartOf rdf:resource="urn:issn:0027-8128"/>
       </bibo:Article>
   </rdf:RDF>

   ...other elements...

   </model>

**3. Extending the previous example so that the Article declaration contains an ordered list of authors.**

.. code-block:: xml

   <?xml version="1.0"?>

   <model xmlns="http://www.cellml.org/cellml/1.1#"
   xmlns:cmeta="http://www.cellml.org/metadata/2.0#"
   cmeta:id="ip3_model"
   name="ip3_model"
   xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
   xmlns:bqmodel="http://biomodels.net/model-qualifiers/"
   xmlns:bibo="http://purl.org/ontology/bibo/"
   xmlns:dcterms="http://purl.org/dc/terms/"
   xmlns:foaf="http://xmlns.com/foaf/0.1/"
   >

   <rdf:RDF>
       <rdf:Description rdf:about="#ip3_model">
           <bqmodel:description rdf:resource="#example_article"/>
       </rdf:Description>

   <bibo:Article rdf:ID="example_article">

       <bibo:authorList>
           <rdf:Seq>	
               <rdf:li rdf:resource="#fred_bagg"/>
           <rdf:li rdf:resource="#joe_fligs"/>
         </rdf:Seq>
       </bibo:authorList>

       <dcterms:issued>1981</dcterms:issued>
       <dcterms:title>Pertubations in calcium signaling activate immune 
       system function</dcterms:title>
       <bibo:volume>66</bibo:volume>
       <bibo:issue>10</bibo:issue>
       <bibo:pageStart>1102</bibo:pageStart>
       <bibo:pageEnd>1111</bibo:pageEnd>
       <dcterms:isPartOf rdf:resource="urn:issn:0027-8128"/>
   </bibo:Article>

   <foaf:Person rdf:ID="fred_bagg" foaf:name="Fred Bagg"/>
   <foaf:Person rdf:ID="joe_fligs" foaf:name="Joe Fligs"/>

   </rdf:RDF>

   ...other elements...

   </model>

**4. The model that a component represents is described in a book chapter which is not in Pubmed.**

.. code-block:: xml

   <?xml version="1.0"?>

   <model xmlns="http://www.cellml.org/cellml/1.1#"
   xmlns:cmeta="http://www.cellml.org/metadata/2.0#"
   cmeta:id="some_model"
   name="some_model"
   xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
   xmlns:bqmodel="http://biomodels.net/model-qualifiers/"
   xmlns:bibo="http://purl.org/ontology/bibo/"
   xmlns:dcterms="http://purl.org/dc/terms/"
   xmlns:foaf="http://xmlns.com/foaf/0.1/"
   >

   ...other elements...

   <component cmeta:id="example_component">

   <rdf:RDF>
       <rdf:Description rdf:about="#example_component">
       <bqmodel:description rdf:resource="#the_chapter"/>
   </rdf:Description>


   <bibo:chapter rdf:ID="the_chapter">
       <dcterms:isPartOf rdf:resource="#the_book"/>
       <dcterms:creator rdf:resource="#sam_smith"/>
       <bibo:chapter>14</bibo:chapter>
   <dcterms:title>Marsh-warbler feeding calls</dcterms:title>
       <bibo:pageStart>160</bibo:pageStart>
       <bibo:pageEnd>164</bibo:pageEnd>
   </bibo:chapter>

   <bibo:EditedBook rdf:ID="the_book">
      <dcterms:publisher rdf:resource="#the_publisher"/>
      <bibo:editorList>
         <rdf:Seq>
         <rdf:li rdf:resource="#hamish_wang"/>
         <rdf:li rdf:resource="#fred_ming"/>
         <rdf:li rdf:resource="#gertrude_brown" />
         </rdf:Seq>
      </bibo:editorList>
      <dcterms:issued>September, 2010</dcterms:issued>
         <bibo:isbn>3273876876K</bibo:isbn>	
   </bibo:EditedBook>

   <foaf:Organisation rdf:ID="the_publisher" foaf:name="Marsh Animals Press" />

   <foaf:Person rdf:ID="hamish_wang" foaf:name="Hamish Wang"/>
   <foaf:Person rdf:ID="fred_ming" foaf:name="Fred Ming"/>
   <foaf:Person rdf:ID="gertrude_brown" foaf:name="Gertrude Brown"/>
   <foaf:Person rdf:ID="sam_smith" foaf:name="Sam Smith"/>

   </rdf:RDF>

   ...other elements...

   </model>

**5. The model that a component represents is described in a presentation performed at a conference**

.. code-block:: xml

   <?xml version="1.0"?>
   <model xmlns="http://www.cellml.org/cellml/1.1#"
   xmlns:cmeta="http://www.cellml.org/metadata/2.0#"
   cmeta:id="some_model"
   name="some_model"
   xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
   xmlns:bqmodel="http://biomodels.net/model-qualifiers/"
   xmlns:bibo="http://purl.org/ontology/bibo/"
   xmlns:dcterms="http://purl.org/dc/terms/"
   xmlns:foaf="http://xmlns.com/foaf/0.1/"
   xmlns:event="http://purl.org/NET/c4dm/event.owl#"
   xmlns:timeline="http://purl.org/NET/c4dm/timeline.owl#"
   >

   ...other elements...

   <component cmeta:id="example_component">

   <rdf:RDF>
       <rdf:Description rdf:about="#example_component">
           <bqmodel:description rdf:resource="#the_presentation"/>
       </rdf:Description>

   <bibo:Slideshow rdf:ID="the_presentation">
       <dcterms:creator rdf:resource="#sam_smith"/>
       <dcterms:date>16-April-2010</dcterms:date>
       <dcterms:title>Marsh Warblers I have known</dcterms:title>
       <bibo:presentedAt rdf:resource="#the_conference" />
   </bibo:Slideshow>

   <foaf:Person rdf:ID="sam_smith" foaf:name="Sam Smith"/>

   <bibo:Conference rdf:ID="the_conference" >
       <event:place rdf:resource="http://sws.geonames.org/2193733/" />
       <timeline:at rdf:datatype="http://www.w3.org/2001/XMLSchema#dateTime">2010-10-25T12:00:00</timeline:at>
       <timeline:duration rdf:datatype="http://www.w3.org/2001/XMLSchema#duration">PT5D</timeline:duration>
       <dcterms:title>Marsh Warbler Symposium 2010</dcterms:title>
   </bibo:Conference>

   </rdf:RDF>

   ...other elements...

   </model>

