.. _cellmlmetaspec-licensing:

===========================================
CellML Licensing Metadata Specification 2.0
===========================================

June 2011

:Authors:
   Michael T. Cooling (m.cooling@auckland.ac.nz)

:Contributors:
   Catherine M. Lloyd

   David P. Nickerson

   Caton Little

**Status of this Document**

Discussion draft.

**Dependencies**

This specification is dependent on the CellML Core Metadata Specification 2.0.

Introduction
============

This document describes how to annotate a CellML model document with licensing information for that model document.

Realisation Strategy
====================

Licensing information should be added using the Dublin Core (http://dublincore.org/documents/2010/10/11/dcmi-terms/) term 'license', as the RDF Predicate, with the model document's <model> tag as the RDF Subject. The Object could be a URI to the licensing information, or a literal text version of the license, or a combination of the two. If both are desired, use of the RDF container 'Alt' is recommended. See http://www.w3.org/TR/rdf-primer/#containers for more information on RDF containers.

It may be desirable to specify a multi-license for the model. If the objects of RDF statements are the alternative licenses, then RDF Container 'rdf:Alt' is not recommended because it is open - it does not specify that those are the ONLY alternatives. The RDF Collection construct may be used, however here the semantic meaning of being alternatives is lost, implying that all licenses should apply. Hence in this situation, it is recommended to define a license document that specifies the multi-license intent and links to the finite number of alternatives. It is that initial document that should be specified as the RDF Object of an annotation statement. The recommended Dublin Core namespace declaration is as follows:

.. code-block:: xml

   xmlns:dcterms="http://purl.org/dc/terms/"

Dublin Core terms not explicitly mentioned above are not considered part of the CellML Licensing Metadata Specification 1.1.
Examples

**1. A URI reference to a license.**

.. code-block:: xml

   <?xml version="1.0"?>
   <model xmlns="http://www.cellml.org/cellml/1.1#"
       xmlns:cmeta="http://www.cellml.org/metadata/2.0#"
       cmeta:id="model_example"
       name="model"
       xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
       xmlns:dcterms="http://purl.org/dc/terms/"
   >

   <rdf:RDF>
       <rdf:Description rdf:about="#model_example">
           <dcterms:license rdf:resource="
           http://exampleweb.org/licenses/2.0/" />
       </rdf:Description>
   </rdf:RDF>

   ... other model elements ...

   </model>

**2. Both a license URI and a literal text of the license (using the rdf:Alt construct)**

.. code-block:: xml

   <rdf:RDF>
   <rdf:Description rdf:about="#model_example">
       <dcterms:license>
           <rdf:Alt>
               <rdf:li rdf:resource="http://exampleweb.org/licenses/2.0/" />
               <rdf:li>
                  Example License
               THE WORK (AS DEFINED BELOW) IS PROVIDED UNDER THE TERMS OF THIS
               LICENSE. THE WORK IS PROTECTED BY COPYRIGHT AND/OR OTHER
               APPLICABLE LAW. ANY USE OF THE WORK OTHER THAN AS AUTHORIZED
               IS...UNAUTHORIZED. 
                </rdf:li>
            </rdf:Alt>
        </dcterms:license>
   </rdf:Description>

