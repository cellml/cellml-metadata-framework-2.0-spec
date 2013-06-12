.. _cellmlmetaspec-core:

================================================
CellML Metadata Framework Core Specification 2.0
================================================

Last Updated: December 2011

:Authors:
  Michael T. Cooling (m.cooling@auckland.ac.nz)

:Contributors:
  Randall Britten
  
  James R. Lawson

  Andrew K. Miller

  David P. Nickerson

  Poul F. M. Nielsen

  Tommy Yu

**Status of this Document**

Discussion draft.

Introduction
============

The CellML Metadata 2.0 Framework describes how annotations should be connected to elements within CellML 1.1 model documents. The framework is designed to be modular. It comprises a Core specification (this document), accompanied by one or more satellite specifications. The satellite specifications are each designed to cater for annotation of models for a specific domain or purpose. Examples include the Citation Specification and the Licensing Specification, which cater for adding metadata about citable works, and licenses pertaining to the model, respectively. The modular specification framework allows great flexibility through the addition of satellite specifications for dealing with new domains of interest, and incremental development of annotation pertaining to specific domains.
Scope of the CellML Metadata Framework

* The CellML Metadata Framework 2.0 is designed specifically for the CellML Language 1.1 (http://www.cellml.org/specifications/cellml_1.1).
* This framework deals with annotations that relate solely to a CellML model document. Annotations that pertain to the model itself (as in the abstract entity held in people's heads), or that depend on the CellML model document in some context (for example, the curation status of the model in some repository, or the use of the model document in some virtual experiment), are not within the scope of this framework.
* In reality, annotations may be made inside, or outside of a CellML model document. This framework deals with those that are made inside a CellML model document. Where annotations are made outside of a model document, consistency with the framework is recommended in order to provide the greatest potential of using framework-compatible tools.
* This metadata specification pertains to adding metadata to CellML elements as per the CellML 1.1 language specification. It is possible to define extension elements in addition to the elements defined in the 1.1 language specification. Metadata attached to extension elements is not explicitly recognised by this framework.

When annotating or attempting to use the annotation provided by this framework, the following points should be observed:

* This framework holds an 'open-world' assumption. That is, not all real-world relationships are necessarily documented as annotation. It is not to be assumed for a given model document that every possible relationship is be annotated.
* Annotation is in no way guaranteed to be correct, or up-to-date. Even assuming an annotation was true when it was made, is no guarantee that the annotation will remain true over any period of time. Nor is a given annotation within a model document guaranteed to be consistent with any other annotation within that document, or in any other document. The CellML Metadata Specification Framework 2.0 details how annotations are to be made but gives no information as to the annotations' validity or truthfulness at a particular point in time. Care should be taken not to assign multiple contradictory annotations to a particular model element, including possible contradictory annotations between parent and children elements.

Satellite Specification Principles
==================================

In order to ensure a consistent and functional framework, the following principles should be borne in mind when defining new, or updating existing, satellite specifications:

* Each satellite metadata specification must be compatible with this Core Metadata Specification.
* Ontologies used for defining relationships in satellite specifications should be derived from existing 'standard' ontologies where possible (as ratified by appropriate communities, where possible).
* Relationship types to be encoded as part of a new specification, or an update to an existing specification, should be chosen so as to be either direct subsets of, or orthogonal to, the relationship types advocated in existing satellite specifications. Other satellite specifications may need to be updated to ensure that this is the case.
* Relationship types should be chosen so as to avoid 'use-mention confusion'. An example of this confusion would be a relationship type 'is a' when used with a CellML component and a database record representing a biological entity. This is incorrect, because a CellML component is not a database record. It is not even a biological entity. In fact, the component represents a biological entity that is represented by the aforementioned database record. While in some limited domains, assumptions of this type are intrinsic and untangling is taken 'as read', errors of this type make working with annotations from multiple ontologies problematic, particularly for machine processing, and should be avoided.
* Where possible, the collection of advocated predicates should be defined. For example, a specific versioned ontology would be preferable to an evolving ontology whose future members maybe be undefined.
* Predicates should be in the form of nouns, where possible. This is for maximum compatibility with the Realisation Strategy (see below). For example 'part' is preferable to 'isPart', since resolving the RDF to English yields 'Subject A has an isPart whose values is Object B' in the first case, which is not as natural as 'Subject A has a part whose value is Object B'.

Realisation Strategy
====================

Annotations will be made using RDF (http://www.w3.org/RDF/) Statements. Briefly, an RDF statement has three parts, a Subject, a Predicate and an Object. The Subject of an annotation will generally be a CellML model element, and the Predicate will generally be some kind of relationship type. The Object will generally be an external entity such as an identifier for a publication, or perhaps a record in a database of known genes. The RDF statement such as that outlined above links a model element to an external entity, thereby providing an annotation. For a conceptual primer on RDF, please see the following web document http://www.w3.org/TR/rdf-primer/.As CellML is serialized as an XML document, Framework annotations will be serialized using RDF/XML (http://www.w3.org/TR/rdf-syntax-grammar/). As per RDF/XML standards, in order for the documents to be valid, namespaces for the XML attributes through which annotations are encoded will need to be declared. Specifically, a core namespace has been defined specifically to encompass annotations made in a CellML model document:

+----------------------------+-------------------------------------------+
| Suggested prefix           | Namespace URI                             |
+============================+===========================================+
| cmeta                      | ``"http://www.cellml.org/metadata/2.0#"`` |
+----------------------------+-------------------------------------------+

(note the "2.0" towards the end of the Namespace URI, which pertains to the 2.0 Metadata Specification, as opposed to "cmeta" as used in the previous 1.0 Metadata Specification [http://www.cellml.org/specifications/metadata/cellml_metadata_1.0]). Additional namespaces for particular relationship types, where required, must be defined in the associated satellite specifications.

Model elements can be referenced by RDF statements if they have an ID. An attribute "cmeta:id" can be created on any CellML element. The "id" is required to be unique across all attributes of type ID inside a CellML model document. Making annotations to MathML inside a CellML model document is similarly catered for using MathML's existing "math:id" optional attribute. The creation of "id"s is covered in more detail in the CellML 1.1 language specification, section 8 (http://www.cellml.org/specifications/cellml_1.1/#sec_metadata).

RDF annotation linking to CellML model elements should be enclosed within RDF tags. A simple example with one set of RDF tags is shown below. The example shows the basic declaration of a CellML model document, the core annotation namespace and RDF tags. A cmeta:id defined on the <model> tag (or any other tag as per above) can be used as a 'hook' to attach annotation to (in this case, the ID is declared is 'model_example').

.. code-block:: xml

   <?xml version="1.0"?>
   <model xmlns="http://www.cellml.org/cellml/1.0#"
          xmlns:cmeta="http://www.cellml.org/metadata/2.0#"
          xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
          cmeta:id="model_example"
          name="model_example" > 

   ...other elements...

   <rdf:RDF>
     <rdf:Description rdf:about="#model_example"> ...annotation about the <model> element goes here </rdf:Description>
   </rdf:RDF>

   ...other elements...

   </model>

 

In keeping with RDF/XML, a basic RDF Subject declaration is shown above, by means of the rdf:Description tag. The rdf:about attribute uses the URI reference fragment delimiter ('#') to declare that the Subject is the element with the ID of 'model_example', within the current document.

For additional examples of the RDF/XML that goes between the RDF tags, please see one or more of the CellML Metadata Framework 2.0 satellite specifications.

An alternative method of attaching metadata to CellML model elements is to use, as values of the rdf:about attribute, URIs with XPATH 1.0 expressions (see http://www.w3.org/TR/1999/REC-xpath-19991116/ for details) as fragments. XPATH is attractive because it can specify not only XML tags that have IDs, but any XML node within the document - such as attributes of tags, or tags without IDs.

The ability to use complex expressions as fragments for URIs that reference XML documents is provided by the XPointer Framework W3C Recommendation (http://www.w3.org/TR/2003/REC-xptr-framework-20030325/). At the time of writing, the part of the XPointer Framework (namely the 'xpointer() Scheme') allowing XPATH expressions is not yet a W3C Recommendation, but a Working Draft (see http://www.w3.org/TR/2002/WD-xptr-xpointer-20021219/ for details). The requirements here are a subset of those catered for by that Draft, and by other xpointer Schemes currently in the xpointer Scheme Registry (http://www.w3.org/2005/04/xpointer-schemes/). Therefore a simpler scheme called the' xpointernode() Scheme' is defined here. The syntax of the xpointernode() Scheme is defined as a subset of XPATH 1.0 expressions (http://www.w3.org/TR/1999/REC-xpath-19991116/) - specifically those that are evaluated to ultimately yield an object of type XPATH 'node-set' (thus excluding those expressions that ultimately evaluate to objects of other XPATH data types), where the node-set contains exactly one element.

Some examples of such fragments as values of rdf:about attributes are::

   rdf:about="#xpointernode(component[@name='calcium'])"

making the component with the name of 'calcium' the RDF Subject of the annotation, and::

   rdf:about="#xpointernode(component
   [@name='calcium']/variable[@name='concentration']/@initial_value)"

making the 'initial_value' attribute of the variable whose name is 'concentration', inside the component with the name of 'calcium', the RDF Subject of the annotation.

Using XPATH expression predicates that rely on a particular ordering of model elements is possible in this Scheme, but strongly discouraged when used for CellML models. Model element order in CellML has no particular meaning and can often change when a model is serialized by different tools, complicating the ability of such tools to keep annotations attached appropriately. Instead, elements should be identified as explicitly as possible within XPATH expressions, by making use of name and/or id attributes where applicable.

Finally, note that the standardised method of using XPATH within URI fragments facilitates the attachment of metadata to CellML model elements outside of the current XML document, for example in the following RDF Subject declaration::
   rdf:about="http://www.cellml.org/somewhere/otherdocument.cellml#xpointernode(component[@name='calcium'])"

which references a particular component in a file accessible on the Web. xpointernode() fragments can also be used with other URI types that also point to XML documents, such as 'file://' URIs.
