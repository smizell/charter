# Elements of a Hypermedia Message

The purpose of this document is to outline the elements of a hypermedia message for use in designing an general interface for accessing hypermedia resources.

The elements found in this document are derived from the most common hypermedia formats in use today.

1. [HAL](http://stateless.co/hal_specification.html)
2. [Collection+JSON](http://amundsen.com/media-types/collection/)
3. [Siren](http://sirenspec.org)
4. [UBER](https://rawgit.com/mamund/media-types/master/uber-hypermedia.html)
5. [Mason](https://github.com/JornWildt/Mason)
6. [HTML](http://www.w3.org/TR/html5/)

To faithfully represent many of the elements of each of these media types, this document will try cover every hypermedia element possible from these formats. A more detailed and specific document for each media type will be done outside of this document.

## Attributes

Attributes are considered to be properties of a resource. In some media types, attributes may be referred to as `properties`. While document types such as XML or JSON provide ways to do more complex types, an attribute is usually consider a name and a value.

### Semantics

Semantics are ways to give meaning to attributes.

1. **Label** - Some formats provide ways to give a human-readable label to an attribute. For example, where `first_name` is the attribute, "First Name" may be the label.
2. **Type** - Used in several media types for pointing to outside semantics, such as Schema.org. This can be seen in documents that focus on linked data, such as JSON-LD and HTML+RDFa.

Semantics may be defined in out-of-band documents, such as the specific media type documentation, link relations, or profiles. Usage of those types of documents are outside the scope of this document.

## Transitions

In essence, a transition is an available progression from one state to another state. There are many characteristics of transitions, and several different categories that hypermedia formats use.

These different types of transitions can be broken down into [Aspects](http://www.slideshare.net/rnewton/amundsen-costbenefitshypermedia/80) and [H-Factors](http://amundsen.com/hypermedia/hfactor/).  This document will primarily look at three different kinds of transitions:

1. Links
2. Actions
3. Link and Action Templates

### Links

Links are a common category of transitions that are considered safe transitions. At minimum, a link has the following.

1. **Relation Type** - Relation type or name of the transition
2. **URI** - URI of the link

#### Queries

A query is a safe link that has parameters that can be added as query string for the URI. These parameters include:

1. **Name** - The name of the parameter
2. **Value** - The value of the parameter
3. **Default Value** - The default value of the parameter

#### Embedded Resources

Many media types provide ways to embed data for linked resources. Some profiles or formats may consider some of this to be link hints

##### Embedded Meta Data, Attributes, and Transitions

Some media types provide ways to embed meta data about the linked resource. This meta data may be links to a profile for the resource, or other relevant links defining how to handle the linked resource.

Many media types also provide ways for embedding attributes and transitions available for the linked resource. An embedded resource MAY be considered to be partially embedded or fully embedded. If fully embedded, it  MUST include all of the data that would be available if the URI had been requested.

##### Anonymous Links and Actions

There are several specs that allow for [link hints](http://tools.ietf.org/html/draft-nottingham-link-hint-00), which allows for providing information about the HTTP methods can be invoked on a URI. While this is providing a way to embed available actions, it does so by merely providing the HTTP methods, and does not provide a name for the relation types.

### Actions

An action is a type of transition that SHOULD be considered unsafe. 

An action has the following:

1. **Relation Type** - Relation type or name of the transition
2. **URI** - URI of the action
3. **Method** - this MUST be unsafe HTTP method for the transition, which include POST, PUT, and DELETE

It also provides a way for defining attributes, which some media types call body parameters or fields. These attributes include:

1. **Name** - The name of the parameter
2. **Value** - The value of the parameter
3. **Default Value** - The default value of the parameter

### Link and Action Templates

Instead of using a normal URI, link and action templates use a URI template  based on [RFC 6570](http://tools.ietf.org/html/rfc6570). While formats like HAL combine links and link templates, it is helpful to keep these conceptually different. 

The parameters for the URI template use the following:

1. **Name** - The name of the parameter
2. **Value** - The value of the parameter
3. **Default Value** - The default value of the parameter

Parameters that are part of the URI path MUST be required.

## Meta

Hypermedia types provide ways to include meta data for both the current resource and linked resources. 

### Attributes

Meta attributes includes data about the resource or linked resource. 

### Links

Meta links are links that provide relevant resources for processing the returned resource. This may include profile links, help links, or even links to styling documents.

## Curies

Curies are ways to shorten URLs based on the [W3 spec](http://www.w3.org/TR/curie/). It includes a shortened name for the curie along with a URI prefix.

## Errors

TBD