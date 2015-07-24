.. _cellmlmetaspec-biological:

=================================================================
CellML Metadata Framework Biological Annotation Specification 2.0
=================================================================

:Authors:
   Michael T. Cooling (m.cooling@auckland.ac.nz)
   
:Acknowledgements:
   Maxwell L. Neal
   
**Dependencies**

This specification is dependent on the CellML Metadata Framework Core Specification 2.0.

Introduction
============
 
This document describes how annotations can be added to elements of a CellML model document that declare what biological entities or processes those elements represent.

Realisation Strategy
====================

To link CellML elements to biological concepts one should use RDF statements where the CellML element is the RDF Subject, and a URI to the concept the RDF Object. The RDF Predicate should be chosen from the Biomodels Biological Qualifiers (at the time of writing, a list can be found here: http://www.ebi.ac.uk/miriam/main/qualifiers/, under the heading 'biology-qualifiers'). In keeping with the principles in the CellML Core Metadata Specification 2.0, the 'noun' forms of the predicates must be used e.g. 'identity' rather than 'is', or 'part' rather than 'hasPart'.

The recommended Biological qualifier namespace is shown in the table below.

+----------------------------+------------------------------------------------+
| Suggested prefix           | Namespace URI                                  |
+============================+================================================+
| bqbiol                     | ``"http://biomodels.net/biology-qualifiers/"`` |
+----------------------------+------------------------------------------------+

In keeping with the Biomodels framework, the RDF Objects of the annotation statements should be URIs to biological concepts (not to instances of the biology themselves). Where possible these should link to a publically accessible ontology (or database, if no suitable ontology can be found) of such concepts. The recommended (sub-)ontologies are the following:
	
   Protein Ontology
   
   UniProt
   
   ChEBI 
   
   Gene Ontology:cell component
   
   Cell Ontology 
  
   Mouse Adult Gross Anatomy
   
   Foundational Model of Anatomy
   
   Ontology of Physics for Biology
 
Where possible, Identifier.org URIs (http://identifiers.org) should be used as the RDF Object URIs, as these are persistent and easily dereferenced.

It is important to try to be as precise as possible. For example, it might seem useful to annotate a CellML component as representing a particular protein. However, if the component also represents other things, then better alternatives might include using 'part' instead, or annotating a specific CellML variable rather than the entire CellML component as being that particular protein. Similarly, it is important to try to be as specific as possible. Annotating a CellML component to the effect that it represents 'a protein' (which one?) is often not as useful as relating it to a particular protein.

Examples
========

**1. A component represents a particular protein**

The protein is listed in the Protein Ontology as ID:000005120.

.. code-block:: xml

       <?xml version="1.0"?>
       <rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
                xmlns:bqbiol="http://biomodels.net/biology-qualifiers/">
                
           <rdf:Description rdf:about="./model.cellml#c">
               <bqbiol:hypernym rdf:resource="http://identifiers.org/obo.pr/PR:000005120" />
           </rdf:Description>
           
       </rdf:RDF>

**2. An equation contains two terms that deal with different biological processes**

The biological processes are represented by Gene Ontology records 0051603 and 0042398.

.. code-block:: xml

   <?xml version="1.0"?>
   <rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
            xmlns:bqbiol="http://biomodels.net/biology-qualifiers/">
   
       <rdf:Description rdf:about="./model.cellml#the_equation">
           <bqbiol:part>
               <rdf:Bag>
                   <rdf:li rdf:resource="http://identifiers.org/obo.go/GO:0051603" />
                   <rdf:li rdf:resource="http://identifiers.org/obo.go/GO:0042398"  />
               </rdf:Bag>
           </bqbiol:part>
       </rdf:Description>
       
   </rdf:RDF>
   