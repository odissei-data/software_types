# Software Types

Schema.org profile for specifying software application types, used for software metadata descriptions.

**Authors**: Maarten van Gompel and Daniel Garijo

**Profile available at**: [https://w3id.org/software-types](https://w3id.org/software-types)

**Supported serializations**: [JSON-LD](software-types.jsonld) (`application/ld+json`), [Turtle](software-types.ttl) (`text/turtle`) and HTML (this readme). See the code snippet below for an example on how to retrieve the profile in Turtle with a `curl` command:

```
curl -sH "accept:text/turtle" -L https://w3id.org/software-types
```

Different releases have their own version. For example:
```
curl -sH "accept:text/turtle" -L https://w3id.org/software-types/1.0.0/
```

## Introduction

This profile describes vocabulary terms needed to describe the metadata of software application types (e.g., command line, desktop, software package, software library, etc.). The profile is meant to be used
with [schema.org](https://schema.org/) and [codemeta](https://codemeta.github.io). Our goal is to introduce
as little additional vocabulary as possible and only extend where schema.org
and codemeta leave representation gaps.

## Why do we need a schema.org profile for software types?

Software applications are ubiquitous in our society, ranging simple desktop applications in personal desktops to packages, libraries or containers deployed in servers. In fact, schema.org partly acknowledges the importance of software by having the term `schema:SoftwareApplication` as the target of `schema:SoftwareSourceCode`. This is because software applications typically offers one or more interfaces through which
users or machines can interact with it. Schema.org includes popular types of ``schema:SoftwareApplication``
like ``schema:WebApplication``, ``schema:MobileApplication``, and even
``schema:VideoGame``. However, we find that schema.org subtypes for software applications are quite lacking when we look at them from a software development perspective. Here we attempt to fill the gaps and define further (sub)types
such as command-line applications, desktops applications (GUIs) and programming libraries.

This availability of explicit software interface type information allows for
more accurate software metadata descriptions. Implementing these as types, in
line with what schema.org already does, allows for further type-specific
refinements. A good example is the
[WebAPI](https://github.com/schemaorg/schemaorg/issues/1423) type that is in
development which allows for describing Web APIs, though not a subtype of
``schema:SoftwareApplication``.

**Disclaimer**: this work aims to create a profile that may be incorporated into codemeta or schema.org. The profile has persistent identifiers, but, if standardized, the classes and properties defined here may be absorbed into other initiatives.

## Software types profile: Classes

We define the following additional classes to describe software based on
interface type. All proposed classes extend `schema:SoftwareApplication`.

* ``CommandLineApplication`` - A software application requiring a command-line interface as the primary means of interaction. Examples are popular tools like: ``grep``, ``sed``, ``git``.
* ``DesktopApplication`` - A software application requiring a desktop graphical user interface. Examples include popular software like Firefox, Microsoft Word, Facetime.
* ``NotebookApplication`` - A web application in the form of a notebook (e.g. Jupyter Notebook, R Notebook) or data story.
* ``ServerApplication`` - A software application running as a daemon providing a service, either locally or over a network, running in the background. Examples are software like nginx, MySQL, postfix
* ``SoftwareImage`` - A software application in the form of an image (such as a container image or virtual machine image) that distributes the application along with its wider dependency context.
* ``SoftwareLibrary`` - A software application offering an Application Programming Interface (API) for developers. Examples include software like openssl, libxml2, blas, Huggingface transformers, python-requests.
* ``SoftwarePackage`` - A software application in the form of a package for any particular package manager. It distributes the application but not necessarily its wider dependency context.
* ``TerminalApplication`` - A software application requiring an interactive terminal text-based user interface. Examples include popular tools like vim, mutt, htop, tmux, ncmpcpp, mc, etc.

Note that the following existing schema.org types are already available for you without needing our extension. Descriptions in *italics* are our own clarifications:

* `schema:WebApplication` - Web applications - *A software application offered through the browser*
* `schema:MobileApplication` - A software application designed specifically to work well on a mobile device such as a telephone. 
* `schema:VideoGame` - A video game is an electronic game that involves human interaction with a user interface to generate visual feedback on a video device.

Though not a subclass of `SoftwareApplication` unlike all the others, the following existing class can be used for webservices:

* `schema:WebAPI` - An application programming interface accessible over Web/Internet technologies

## Software types profile: Properties

### Executable name

The base filename of the executable for the software application. It should not
be a full path, nor should it contain any command-line parameters.

We include this property to make a clear distinction between the human readable
name of the software, and the executable used in invocation of the software.
The two may regularly differ with one being more verbose or have stricter
casing than the other.

Examples of this property are also shown in the code snippets A and B. 

It's recommended to either leave out any platform-specific extensions like
``.exe`` if the executable differs across platforms, or to simply use the
property multiple times to list all possible variants.

## How are software types terms used with codemeta?

Codemeta, building upon schema.org, describes software metadata focused on the
software's source code (`schema:SoftwareSourceCode`). We various artefacts that 
can be produced by the source code are linked to it using the  `codemeta:isSourceCodeOf` property (see discussion
[here](https://github.com/codemeta/codemeta/issues/267)). These target products
take one of the types defines in our profile, or one of the existing ones
already in schema.org.

**Note:** In Codemeta v2, we used the `schema:targetProduct` property for this because `codemeta:isSourceCodeOf` wasn't introduced yet before v3.

Example A (JSON-LD): An application named [WIDOCO](https://github.com/dgarijo/Widoco/) is both a command line application in Java (JAR), but also a library:

```json
{
    "@context": [
        "https://w3id.org/codemeta/3.0",
        "https://raw.githubusercontent.com/schemaorg/schemaorg/main/data/releases/13.0/schemaorgcontext.jsonld",
        "https://w3id.org/software-types"
    ],
    "@type": "SoftwareSourceCode",
    "name": "WIDOCO",
    "version": "1.14.17",
    "codeRepository": "https://github.com/dgarijo/Widoco",
    ...,
    "isSourceCodeOf": [
        {
            "type": "CommandLineApplication",
            "name": "WIDOCO",
            "executableName": "Widoco-1.14.17-jar-with-dependencies.jar",
            "runtimePlatform": "Linux"
        },
        {
            "type": "SoftwareLibrary",
            "name": "WIDOCO",
            "runtimePlatform": "Linux"
        },
    ]
}
```

Example B: A python package ([Chowlk](https://github.com/oeg-upm/Chowlk)) can be run as a web service, online (note that example links are provided):

```json
{
    "@context": [
        "https://w3id.org/codemeta/3.0",
        "https://raw.githubusercontent.com/schemaorg/schemaorg/main/data/releases/13.0/schemaorgcontext.jsonld",
        "https://w3id.org/software-types"
    ],
    "@type": "SoftwareSourceCode",
    "name": "Chowlk",
    "codeRepository": "https://github.com/oeg-upm/Chowlk",
    ...,
    "isSourceCodeOf": [
        {
            "type": "WebApplication",
            "executableName": "chowlk-webapp",
            "provider": {
                "@type": "Organization",
                "name": "Ontology Engineering Group"
            }
            "url": "https://example.org/chwolk-ws"
        },
        {
            "type": "WebAPI",
            "provider": {
                "@type": "Organization",
                "name": "Ontology Engineering Group"
            }
            "endpointUrl": {
                "@type": "EntryPoint",
                "url": "https://example.org/chowlk-service",
                "contentType": "application/json"
            },
            "endpointDescription": {
                {
                  "@type": "CreativeWork",
                  "encodingFormat": "application/json",
                  "url": "https://example.org/chowlk-service/info?version=v1"
                },
            },
            "conformsTo": "https://jsonapi.org/format/1.0/",
            "documentation": "https://example.org/chowlk-service/docs",
        },
    ]
}
```

## Software as a Service 

The vocabulary we propose here allows to express a link between software
metadata and instances of that software running as a service somewhere. Example B shows ``schema:WebAPI`` and ``schema:WebApplication`` used to
describe instances of software as a service. We use ``schema:WebAPI`` with a
proposed extension as formulated
[here](https://webapi-discovery.github.io/rfcs/rfc0001.html) and discussed
[here](https://github.com/schemaorg/schemaorg/issues/1423). These are **not** new properties proposed in this profile.

The link between `SoftwareSourceCode` and subclasses of `SoftwareApplication`,
`WebAPI`, `WebPage` or `WebSite` is established using the `isSourceCodeOf`
property. There is also a reverse property for this called
``codemeta:hasSourceCode`` (see discussion
[here](https://github.com/codemeta/codemeta/pull/229)) that can be used in case of need.


## Implementations

Support for our `software_types` extension to codemeta/schema.org is
implemented in the following software:

* [codemetapy](https://github.com/proycon/codemetapy) - Python library and command-line tool for converting to codemeta and creating/manipulating existing codemeta descriptions.
* [codemeta-harvester](https://github.com/proycon/codemeta-harvester) - Automatically harvests and converts software metadata to codemeta
* [somef](https://github.com/KnowledgeCaptureAndDiscovery/somef) ([work in progress](https://github.com/KnowledgeCaptureAndDiscovery/somef/issues/486)) - A Python package for automated software metadata extraction. 
* [c2t](https://github.com/osoc-es/c2t) ([work in progress](https://github.com/osoc-es/c2t/issues/34)), a python package for converting software images into triples (extracting their dependencies)

## Related approaches

Many vocabularies exist to describe software or its constituent parts, e.g., the [software description ontology](https://w3id.org/okn/o/sd/), [description of a project vocabulary](http://usefulinc.com/ns/doap#), [hydra](https://www.hydra-cg.com/spec/latest/core/) (for API description), the common workflow language (description of inputs and outputs of software components, etc.), etc.  Our proposed profile does not aim to redefine any new term related to software, but propose a lightweight profile that can be easily incorporated into schema.org or codemeta.

## Real Examples

You can consult the following projects as examples that make use of this profile:

* [frog](https://github.com/LanguageMachines/frog/blob/master/codemeta.json)
* [ucto](https://github.com/LanguageMachines/ucto/blob/master/codemeta.json)
* [libfolia](https://github.com/LanguageMachines/libfolia/blob/master/codemeta.json)

Furthermore, the [CLARIAH Tools Portal](https://tools.dev.clariah.nl/) is build on the aforementioned implementations and may offer further examples of codemeta that also incorporates this software application type profile.

## Acknowledgement

This work was indirectly and partially funded through the [CLARIAH-PLUS project](https://clariah.nl).

This work has been supported by the Madrid Government (Comunidad de Madrid-Spain) under the Multiannual Agreement with Universidad Politécnica de Madrid in the line Support for R&D projects for Beatriz Galindo researchers, in the context of the V PRICIT (Regional Programme of Research and Technological Innovation)

## Versions

The development version of software types is what you will find in the root folder of this repository. Different stable versions can be found on the `releases` folder.
