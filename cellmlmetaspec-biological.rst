.. _cellmlmetaspec-biological:

=======================================================
CellML Biological Annotation Metadata Specification 2.0
=======================================================

Last Updated: June 2013

:Authors:
   Michael T. Cooling (m.cooling@auckland.ac.nz)
   
:Contributors:
   Maxwell Neal
   
**Status of this Document**

Discussion draft.

**Dependencies**

This specification is dependent on the CellML Core Metadata Specification 2.0.

Introduction
============
 
This document describes how annotations can be added to elements of a CellML model document that declare what biological entities or processes those elements represent.

Realisation Strategy
====================

To link CellML elements to biological concepts one should use RDF statements where the CellML element is the RDF Subject, and a URI to the concept the RDF Object. The RDF Predicate should be chosen from the Biomodels Biological Qualifiers (at the time of writing, a list can be found here: http://www.ebi.ac.uk/miriam/main/qualifiers/, under the heading 'biology-qualifiers'). In keeping with the principles in the CellML Core Metadata Specification 2.0, the 'noun' forms of the predicates must be used e.g. 'identity' rather than 'is', or 'part' rather than 'hasPart'.

The recommended Biological qualifier namespace is as follows:

.. code-block:: xml

   xmlns:bqbio="http://biomodels.net/biology-qualifiers/"

In keeping with the Biomodels framework, the RDF Objects of the annotations statements should be URIs to biological concepts (not to instances of the biology themselves). Where possible these should link to a publically accessible ontology (or database, if no suitable ontology can be found) of such concepts. At the time or writing, we recommend the following ontologies, or sub-ontologies:

   UniProt 
   
   ChEBI 
   
   Gene Ontology:cell component
   
   Cell Ontology 
  
   Mouse Adult Gross Anatomy
   
   Foundational Model of Anatomy
   
   Ontology of Physics for Biology
 
Where possible, Identifier.org URIs (http://identifiers.org) should be used as the RDF Object URIs, as these are persistent and easily dereferenced.

It is important to try to be as precise as possible. For example, it might seem useful to annotate a CellML component as representing a particular protein. However, if the component also represents other things, then better alternatives might include using 'part' instead, or annotating a specific CellML variable rather than the entire CellML component as being that particular protein. Similarly, it is important to try to be as specific as possible. Annotating a CellML component to the effect that it represents 'a protein' (which one?) is often not as useful as relating it to a particular protein.
Annotating Imported Model Elements

It is often desirable to annotate model elements that are part of the model via CellML <import>s. Units and components that are imported to the model can be referenced in RDF statements via their <units> and <component> children (respectively) of the <import> tag where the import is declared. These child tags can be considered as proxies for the actual unit and component definition, hence it is acceptable to consider them as possibly representing biological concepts and Biomodels Biological qualifiers can be used as RDF Predicates in statements about them.

Variables in imported components, whether they are accessible in the CellML sense via their CellML interfaces or not, are not given proxies in the current CellML model document. The only potential reference to them in the model is as part of one or more 'map_variable' elements in <connection>s, which themselves do not represent biology but are pointers to representations of biology. Therefore, RDF statements about the imported variable cannot be validly made using Biomodels Biological qualifiers as predicates. Therefore it is not possible to reference such variables in this Specification.
Examples

**1. A component represents a particular protein (listed in the Protein Ontology ID:000005120)**

.. code-block:: xml

   <model xmlns="http://www.cellml.org/cellml/1.1#"
   xmlns:cmeta="http://www.cellml.org/metadata/2.0#"
   cmeta:id="example_model"
   name="the_model"
   xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
   xmlns:bqbio="http://biomodels.net/biology-qualifiers/"
   >

   ...other elements...

   <component cmeta:id="c" name="c_component">
       <rdf:RDF>
           <rdf:Description rdf:about="#c">
               <bqbio:hypernym
               rdf:resource="http://identifiers.org/obo.pr/PR:000005120" />
           </rdf:Description>
       </rdf:RDF>

   ...other elements...

   </component>

   ...other elements...

   </model>

**2. An equation contains two terms that deal with different biological processes (that are represented by Gene Ontology records 0051603 and 0042398).**

.. code-block:: xml

   <?xml version="1.0"?>

   <model xmlns="http://www.cellml.org/cellml/1.1#"
   xmlns:cmeta="http://www.cellml.org/metadata/2.0#"
   cmeta:id="example_model"
   name="the_model"
   xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
   xmlns:bqbio="http://biomodels.net/biology-qualifiers/">

   ...other elements...

   <math id="the_equation" xmlns="http://www.w3.org/1998/Math/MathML">

   ...other elements...

   <rdf:RDF>
       <rdf:Description rdf:about="#the_equation">
           <bqbiol:part>
               <rdf:Bag>
                   <rdf:li 
                   rdf:resource="http://identifiers.org/obo.go/GO:0051603" />
                   <rdf:li 
                   rdf:resource="http://identifiers.org/obo.go/GO:0042398"  />
               </rdf:Bag>
           </bqbiol:part>
       </rdf:Description>
   </rdf:RDF>

   ...other elements...

   </math>

   ...other elements...

   </model>

Note: specifying exactly which terms of an equation encoded in MathML represent which biological processes may be achieved by making <apply> blocks the RDF Subjects, using the xpointernode() Scheme as described in the CellML Core Metadata Specification 2.0.
