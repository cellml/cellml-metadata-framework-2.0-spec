.. _cellmlmetaspec-basicinfo:

===================================================================
CellML Metadata Framework Basic Model Information Specification 2.0
===================================================================

:Authors:
   Michael T. Cooling (m.cooling@auckland.ac.nz)

:Acknowledgements:
   David Brooks,
   Richard Christie,
   David P. Nickerson,
   Tommy Yu

**Dependencies**

This specification is dependent on the CellML Metadata Framework Core Specification 2.0.

Introduction
============

Regardless of the type of model encoded in a CellML model document, there are three attributes that apply:

* Every model document has a creator (whether human or by some other entity / organisation or artifact). 
* Every model document is created at a point in time.
* Every model document will be created for some reason or purpose, of varying significance (for example: as part of a scientific mode of enquiry, or as a side-effect of some automated process).

The same could be said about any CellML model element. The creator may be part of an organization and recording that relationship may also be desirable. In addition, it is useful to be able to add descriptive, free-form text annotation to model elements for the general purpose of adding comments about them.

This ability to add author, timestamp information and a comment or description is also extended to annotation elements. One could add annotation forming a 'comment' about a piece of CellML code, then annotate that comment to form a comment about that comment etc.

Together, these annotations are considered 'Basic Model Information'. This document describes how these should be annotated for a given CellML model document.

Realisation Strategy
====================

People, organizations and other entities (such as software packages or physical artefacts capable of performing some action) will be described using a subset of an external emerging (currently version 0.1) standard known as Friend-Of-A-Friend (FOAF). At the time of writing, the current FOAF specification (Vocabulary Specification 0.98 "Marco Polo Edition") can be found here: http://xmlns.com/foaf/spec/20100809.html.

In FOAF, the aforementioned entities are collectively known as Agents. CellML model document annotations will describe Agents using RDF-formatted FOAF objects. Most often, a FOAF object will become the RDF Objects of some RDF Subject-Predicate-Object triple. For example: the Agent which creates a model or model element.

In order to describe Agents, the following subset of the FOAF Core is considered part of this specification

FOAF objects

* Person - describing a person
* Agent - superclass of Person and Group. May also describe other types of Agents not given their own explicit subclass, such as software packages
* Group - A collective of Agents

FOAF properties

* name - a name for one of the FOAF objects above
* familyName - a family name, used for Person objects
* givenName - a given name, used for Person objects
* member - used to declare that some Agent is a member of a Group

The relationship to specify the creator of a model document can be made by forming an RDF triple where the Subject is the model tag (via a cmeta:id), the Object is the Agent who is the creator, and the Predicate is described using the 'maker' property from FOAF.

FOAF properties and objects not listed above are not considered part of the CellML Metadata Framework Basic Model Information Specification 2.0.

The point in time at which a model element was created should be encoded using the Dublin Core (http://dublincore.org/documents/2010/10/11/dcmi-terms/) term 'created', used similarly to 'maker' above. The time point itself should be encoded in the W3C's Time and Date Formats Specification (http://www.w3.org/TR/NOTE-datetime). This allows the specification of the time point at an encoder-chosen level of precision.

A general comment on a model element, including, potentially, the purpose of a model (which would be encoded with the cmeta:id of the CellML <model> tag as the RDF Subject), should be encoded using the Dublin Core 'description' term, as text.

The recommended namespaces for using FOAF and the Dublin Core in the context of this specification are as follows:

+----------------------------+-------------------------------------------+
| Suggested prefix           | Namespace URI                             |
+============================+===========================================+
| foaf                       | ``"http://xmlns.com/foaf/0.1/"``          |
+----------------------------+-------------------------------------------+
| dcterms                    | ``"http://purl.org/dc/terms/"``           |
+----------------------------+-------------------------------------------+

Dublin Core terms not explicitly mentioned above are not considered part of the CellML Metadata Framework Basic Model Information Specification 2.0.

In order to make comments about annotation elements, including other comments, RDF 'reification' is used. Conceptually, this involves making the RDF statement that links some model element with an annotation into a statement with an identifier that can itself be referenced in further RDF statements. In pure RDF, this involves making a set of 4 RDF statements (known as an 'RDF reification quad') that together describe the original annotation statement and provide it with an identifier (more details on reification can be found in http://www.w3.org/TR/rdf-primer/). This is in addition to the RDF that actually makes the original annotation statement. In pure RDF this can become verbose, particularly if the original annotation statement has a lengthy Object. Fortunately, in XML/RDF there is a shorthand version, where predicates are assigned an rdf:ID, which can then be used as the RDF Subject of the comment annotation. This specification recommends that approach.

Examples
========

**1. Representing a person, a group and a software package**

.. code-block:: xml

   <?xml version="1.0"?>
   <rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
            xmlns:foaf="http://xmlns.com/foaf/0.1/">

       <foaf:Person>
           <foaf:givenName>Joe</foaf:givenName>
           <foaf:familyName>Bloggs</foaf:familyName>
       </foaf:Person>

       <foaf:Group>
           <foaf:name>Auckland Bioengineering Institute</foaf:name>
       </foaf:Group>

       <foaf:Agent>
           <foaf:name>CellML API v1.8</foaf:name>
       </foaf:Agent>
       
   </rdf:RDF>

**2. Specifying members of a group**

This could be done 'inline' as follows:

.. code-block:: xml

   <?xml version="1.0"?>
   <rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
            xmlns:foaf="http://xmlns.com/foaf/0.1/">
            
       <foaf:Group>
           <foaf:name>Auckland Bioengineering Institute</foaf:name>
           <foaf:member>
               <foaf:Person>
                   <foaf:name>Joe Bloggs</foaf:name>
               </foaf:Person>
            </foaf:member>
       </foaf:Group>
       
   </rdf:RDF>

Or, where an agent might be involved in several annotations within the CellML model document it is recommended to define the Agent separately and use an rdf:nodeID as follows:

.. code-block:: xml

   <?xml version="1.0"?>
   <rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
            xmlns:foaf="http://xmlns.com/foaf/0.1/">
            
       <foaf:Person rdf:nodeID="joe_bloggs">
           <foaf:givenName>Joe</foaf:givenName>
           <foaf:familyName>Bloggs</foaf:familyName>
       </foaf:Person>

       <foaf:Group>
          <foaf:name>Auckland Bioengineering Institute</foaf:name>
          <foaf:member rdf:nodeID="joe_bloggs"/>
       </foaf:Group>
       
   </rdf:RDF>

**3. Attributing creator, timestamp and purpose descriptions to a CellML model**

.. code-block:: xml

   <?xml version="1.0"?>
   <rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
            xmlns:foaf="http://xmlns.com/foaf/0.1/"
            xmlns:dcterms="http://purl.org/dc/terms/">

       <foaf:Person rdf:nodeID="joe_bloggs">
           <foaf:givenName>Joe</foaf:givenName>
           <foaf:familyName>Bloggs</foaf:familyName>
       </foaf:Person>

       <rdf:Description rdf:about="./model.cellml#model_example">
           <foaf:maker  rdf:nodeID="joe_bloggs"/>
           <dcterms:created rdf:datatype="http://purl.org/dc/terms/W3CDTF">2011-02</dcterms:created>
           <dcterms:description>
               This model was constructed as an example model for the CellML Metadata Specification Framework.
           </dcterms:description>
   </rdf:Description>

   </rdf:RDF>

The above example shows the construction of a FOAF Person object, which becomes the RDF subject of a 'maker' relationship for the CellML model document. The 'created' predicate is used to specify that this particular model was created during February 2011, and the 'description' predicate describes the purpose of the model's creation. In the above example all three 'Basic model information' statements are made together, which is recommended, but there is no reason why one or more cannot be absent, or specified as separate statements in the annotation document.

**4. Adding creator and timestamp elements to a model element**

Here we assume that the model element we wish to annotate is defined in the "model.cellml" file as

.. code-block:: xml

   <component name="model_parameters" cmeta:id="parameters">
   
      ...other elements...
   
   </component>

An RDF Description to annotate the component with the cmeta:id of "parameters" could be (using the FOAF Person defined in example 2)

.. code-block:: xml

   <?xml version="1.0"?>
   <rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
            xmlns:foaf="http://xmlns.com/foaf/0.1/"
            xmlns:dcterms="http://purl.org/dc/terms/">
            
       <rdf:Description rdf:about="./model.cellml#parameters">
       <foaf:maker>joe_bloggs</foaf:maker>
           <dcterms:created rdf:datatype="http://purl.org/dc/terms/W3CDTF">2010-11-07</dcterms:created>
       </rdf:Description>
   
   </rdf:RDF>

**5. Adding a text comment to a model element**

The variable for this example, and for examples 6 and 7, would be defined in a cellml model (in a "model.cellml" file) as follows

.. code-block:: xml

   <variable cmeta:id="vi_variable" initial_value="0.025" name="vi" public_interface="out" units="flux"/>

The annotation could be as follows

.. code-block:: xml

   <?xml version="1.0"?>
   <rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
            xmlns:dcterms="http://purl.org/dc/terms/">

       <rdf:Description rdf:about="./model.cellml#vi_variable">
           <dcterms:description>This value of 0.025 comes from Fig 3 caption, page 9110 of the original paper</dcterms:description>
       </rdf:Description>
           
   </rdf:RDF>

**6. Annotating a comment**

Here the comment annotation "vi_comment" is itself annotated with a creator, timestamp and text comment of its own.

.. code-block:: xml

   <?xml version="1.0"?>
   <rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
            xmlns:foaf="http://xmlns.com/foaf/0.1/"
            xmlns:dcterms="http://purl.org/dc/terms/">
            
       <rdf:Description rdf:about="./model.cellml#vi_variable">
           <dcterms:description rdf:ID="vi_comment">
               This value of 0.025 comes from Fig 3 caption, page 9110 of the original paper
           </dcterms:description>
       </rdf:Description>

       <rdf:Description rdf:about="#vi_comment">
           <foaf:maker rdf:nodeID="joe_bloggs"/>
               <dcterms:created rdf:datatype="http://purl.org/dc/terms/W3CDTF">2010-11-05</dcterms:created>
               <dcterms:description>
                   Original author confirms Fig 3 is the best one to use.
               </dcterms:description>
       </rdf:Description>
           
   </rdf:RDF>


Note that in this example the timestamp relates to the first comment (with an rdf:ID of "vi_comment") only, and gives no information as to when the second ("Original author confirms...") was made. If that second comment was itself given a nodeID, it could be further annotated with that information if desired.

**7. A variable with a commented timestamp**

Here the "vi_variable" variable is annotated with a timestamp, which in turn is commented with the timestamper, and a text comment

.. code-block:: xml

   <?xml version="1.0"?>
   <rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
            xmlns:foaf="http://xmlns.com/foaf/0.1/"
            xmlns:dcterms="http://purl.org/dc/terms/">
            
       <rdf:Description rdf:about="./model.cellml#vi_variable">
           <dcterms:created rdf:ID="vi_timestamp" rdf:datatype="http://purl.org/dc/terms/W3CDTF">2010-11-05</dcterms:created>
       </rdf:Description>
           
       <rdf:Description rdf:about="#vi_timestamp">
           <foaf:maker rdf:nodeID="joe_bloggs"/>
           <dcterms:description>This date may be plus or minus 2 days</dcterms:description>
       </rdf:Description>
           
   </rdf:RDF>

