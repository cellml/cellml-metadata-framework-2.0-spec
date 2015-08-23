.. _cellmlmetaspec-core:

================================================
CellML Metadata Framework Core Specification 2.0
================================================

:Authors:
  Michael T. Cooling (m.cooling@auckland.ac.nz)

:Acknowledgements:
  Randall Britten,
  James R. Lawson,
  Andrew K. Miller,
  David P. Nickerson,
  Poul F. M. Nielsen,
  Tommy Yu

Introduction
============

The CellML Metadata 2.0 Framework describes how annotations should be connected to CellML 1.1 model document elements. The framework is designed to be modular. It comprises a Core specification (this document), accompanied by one or more satellite specifications. The satellite specifications are each designed to cater for annotation of models for a specific domain or purpose. Examples include the Citation Specification and the Licensing Specification, which cater for metadata about citable works, and licenses pertaining to the model, respectively. The modular specification framework allows great flexibility through the addition of satellite specifications for dealing with new domains of interest, and incremental development of annotation pertaining to specific domains.

Scope of the CellML Metadata Framework
======================================

* The CellML Metadata Framework 2.0 is designed specifically for the CellML Language 1.1 (http://www.cellml.org/specifications/cellml_1.1).
* This framework deals with annotations that relate solely to a CellML model document or its elements. Annotations that pertain to the model itself (as in the abstract entity held in people's heads), or that depend on the CellML model document in some context (for example, the curation status of the model in some repository, or the use of the model document in some virtual experiment), are not within the scope of this framework.

When annotating or attempting to use the annotation provided by this framework, the following points should be observed:

* This framework holds an 'open-world' assumption. That is, not all real-world relationships are necessarily documented as annotation. It should not be assumed that every possible relationship is be annotated for a given model document.
* Annotation is not, in general, guaranteed to be correct, or up-to-date. Even assuming an annotation was true when it was made, there is no guarantee that the annotation will remain true over any period of time. Nor is a given annotation about some element of a model document guaranteed to be consistent with any other annotation about any other elements in that document, or in any other document. The CellML Metadata Specification Framework 2.0 details how annotations are to be made but gives no information as to the annotations' validity or truthfulness at a particular point in time. Care should be taken not to allow multiple contradictory annotations about a particular model element to become relevant, including possible contradictory annotations between parent and children elements.

Realisation Strategy
====================

Annotations will be made using RDF (http://www.w3.org/RDF/) triples. For a conceptual primer on RDF, please see the W3C RDF Primer (http://www.w3.org/TR/rdf-primer/). Briefly, an RDF statement has three parts, a Subject, a Predicate and an Object. The Subject of an annotation will generally be a CellML model element, and the Predicate will generally be some kind of relationship type. The Object will generally be an external entity such as an identifier for a publication, or perhaps a record in a database of known genes. An RDF statement typically links a model element to an external entity via some relationship, thereby providing an annotation that the model element has that relationship.  

There are several common formats for RDF statements. In keeping with CellML, which is serialized as an XML document, it is recommended that framework annotations be serialized using RDF/XML (http://www.w3.org/TR/rdf-syntax-grammar/). Even within RDF/XML, there are multiple valid ways to express the same relationship. This Metadata Framework documentation typically demonstrates a single simple method for each example, but often there are several alternatives that are equally valid.

In contrast to the CellML Metadata Specification 1.0, in 2.0 annotations should never be made within the CellML model document to which they pertain. Perhaps the simplest (but not the only) way to do this is to place the annotation in a separate XML document, housed its own file, which in turn has the '.xml' extension - assuming the format is RDF/XML.

Model elements can be referenced by RDF statements if they have an appropriate ID. An attribute "cmeta:id" can be created on any CellML element inside its CellML model document. The "id" is required to be unique across all attributes of type ID inside a CellML model document. Making annotations to MathML inside a CellML model document is similarly catered for using MathML's existing "math:id" optional attribute. The creation of "id"s is covered in more detail in the CellML 1.1 language specification, section 8 (http://www.cellml.org/specifications/cellml_1.1/#sec_metadata).

RDF annotation linking to CellML model elements should be enclosed within RDF tags. A simple example is shown below. The example presupposes that there exists, in the same folder as the annotation file, a CellML model document "model.cellml" that has, on its <model> tag, a cmeta:id "model_example" defined. That provides a hook for the RDF Subject to "point to". In this case the model "model_example" is annotated with a simple description, following the CellML Metadata Framework Basic Model Information Specification 2.0.

.. code-block:: xml

   <?xml version="1.0"?>
   <rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
            xmlns:dcterms="http://purl.org/dc/terms/">
            
       <rdf:Description rdf:about="./model.cellml#model_example">
           <dcterms:description>This is an example description that the model_example is annotated with.</dcterms:description>
       </rdf:Description>
       
   </rdf:RDF>

For additional examples of possible annotations, please see the CellML Metadata Framework 2.0 satellite specifications.

As per RDF/XML standards, in order for the documents to be valid, namespaces for the XML attributes through which annotations are encoded will need to be declared. Specifically, a core namespace has been defined specifically to facilitate annotations made in a CellML model document:

+----------------------------+-------------------------------------------+
| Suggested prefix           | Namespace URI                             |
+============================+===========================================+
| cmeta                      | ``"http://www.cellml.org/metadata/2.0#"`` |
+----------------------------+-------------------------------------------+

Annotations may also validly be facilitated with the "cmeta" namespace as defined in the CellML Metadata 1.0 Specification (https://www.cellml.org/specifications/metadata/cellml_metadata_1.0). This means that Metadata Framework 2.0 annotations could be made about CellML models and their elements without modification of the CellML, if Metadata 1.0 cmeta:ids already exist. Care should be taken that all annotations intended for processing remain relevant and are not internally inconsistent when taken together.

It is often desirable to annotate model elements that are part of the model via CellML <import>s. Units and components that are imported into a model can be referenced in RDF statements via their <units> and <component> children (respectively) of the CellML <import> tag where the import is declared. These child tags can be considered as proxies for the actual unit and component definition, which otherwise exist only virtually. Variables in imported components, whether they are accessible in the CellML sense via their CellML interfaces or not, are not given proxies any CellML model document. The only potential reference to them in the model is as part of one or more 'map_variable' elements in <connection>s. If these are given cmeta:ids, care should be taken with the predicates used - it is important to bear in mind that since the mapping is being annotated the annotation is one level removed from the variable itself.

Satellite Specification Principles
==================================

In order to ensure a consistent and functional framework, the following principles should be borne in mind when defining new, or updating existing, satellite specifications:

* Each satellite metadata specification must be compatible with this Core specification.
* Ontologies used for defining relationships in satellite specifications should be derived from existing 'standard' ontologies, as ratified by appropriate communities, where possible.
* Relationship types to be encoded as part of a new specification, or an update to an existing specification, should be chosen so as to be either direct subsets of, or orthogonal to, the relationship types advocated in existing satellite specifications. Other satellite specifications may need to be updated to ensure that this is the case.
* Relationship types should be chosen so as to avoid 'use-mention confusion'. An example of this confusion would be a relationship type 'is a', defined literally, when used with a CellML component and a database record representing a biological entity. This would be incorrect, because a CellML component is not a database record. It is not even a biological entity. In fact, the component represents a biological entity that is represented by the aforementioned database record. While, in some limited domains, assumptions of this type are intrinsic and untangling is taken 'as read', errors of this type make working with annotations from multiple ontologies problematic and should be avoided.
* Where possible, the collection of advocated predicates should be defined. A specific versioned ontology would be preferable to an evolving ontology whose future members maybe be undefined.
* Predicates should be in the form of nouns, where possible. This is for maximum compatibility with the Realisation Strategy (see below). For example 'part' is preferable to 'isPart', since resolving the RDF to English yields 'Subject A has an isPart whose values is Object B' in the first case, which is not as natural as 'Subject A has a part whose value is Object B'.
* Additional namespaces (such as "dcterms" above) for constructs such as particular relationship types (predicates) or objects, where required, must be detailed in the appropriate Metadata Framework satellite specification.

