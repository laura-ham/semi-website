---
layout: article-start
title: Create an ontology
description: 
topic: How To Guides
tags: ['weaviate', 'ontology']
video-link: 
video-caption: 
menu-order: 2
open-graph-type: article
---

## Index

- [What is an ontology and why do you need it?](#what-is-an-ontology-and-why-do-you-need-it)
- [How to build an ontology?](#how-to-build-an-ontology)
	- [Steps to build an ontology](#steps-to-build-an-ontology)
	- [Create an ontology for Weaviate](#create-an-ontology-for-weaviate)
		- [Properties](#properties)
		- [Example](#example)
- [Design tips](#design-tips)
	- [Schema structure](#schema-structure)
	- [Naming](#naming)


## What is an ontology and why do you need it?

In computer science, an ontology is an explicit description of concepts in a domain (classes), with properties describing features, attributes, and interrelationships of the entities. As a decentralized ecosystem, Weaviate connects graph-based instances, where data is semantically stored. Semantic schemas or ontologies form the backbone of a semantic network and are required to link data in Weaviate. A well-formulated ontology is not only valuable for describing the meaning of data within Weaviate instances, it also enables linking data between them. Because ontologies of multiple Weaviate instances share structural elements and semantic definitions, they are ready for data enrichment.

An ontology and a set of data instances form a knowledge base. To initialize a Weaviate instance for your knowledge base, all the data should be structured according to the semantic schema you created. A semantic schema supported by Weaviate consists of a list of classes in a domain of discourse, with properties for each concept describing features and attributes of the concept. Properties have datatype restrictions and may be references to other classes. Weaviate ontologies typically do not have a class hierarchy, which enables usage in multiple domains. 


## How to build an ontology

Developing an ontology includes defining classes, properties and describing allowed values and references for the properties. The structure of the ontology is simple and straightforward as described, but building an ontology from scratch can be challenging. Suggested is to take an iterative approach to ontology development. It is important to keep in mind that an ontology is a model of the reality of the world, so the classes should describe concepts and objects that reflect this reality (in your domain). 


### Steps to build an ontology 
(Inspired by [Ontology Development 101](https://protege.stanford.edu/publications/ontology_development/ontology101.pdf)):

1. **Determine the scope of the ontology.**
The domain of the ontology won’t be hard to define assuming you are going to describe an existing dataset. It is, however, important to keep the purpose of the ontology in mind. What kind of information should be in the ontology to provide answers to the questions you will ask?
2. **Consider reusing existing ontologies.**
There are open source libraries of reusable ontologies and knowledge bases available. Be inspired by the structure and definitions of names in these ontologies. Assuming that have your own (non-semantic) dataset as a basis, you might be able to enrich existing ontologies with your classes.
3. **Define the classes.**
The important terms, objects and concepts in your domain are classes. Give them a name and a definition. 
4. **Define the properties of classes.**
The properties are the semantics of the instances in a database. Properties have a name, a description and also a description of possible data value formats. Supported data types are integers, numbers, date, boolean and string. Additionally, cross-references to classes are also supported as datatype for a property.
5. **Complement with keywords.**
Keywords are used to determine the context of the word, based on the location of the word in the vector space.
Keywords are always stored in an array containing a weight. The keyword itself should be available in the vector space. The weights are -based on the chosen algorithm- used to determine the location of the class or property that the word depicts.


### Create an ontology for Weaviate

Every Weaviate instance needs to have two ontologies (also called "Semantic Schemas"), one for Things and one for Actions. The Thing schemas are used to define actual things. The Action schemas are used to define actions. These actions will also be made available to the things. For example, to execute a command.

Ontologies are always class- and property-based, and classes and properties are enriched by keywords. Ontologies are defined in a <abbr>JSON</abbr> file and should contain the following information:

| Key                                 | Type     | Mandatory? | Value         |
| ----------------------------------- |:--------:|:----------:| ------------- |
| `@context`                          | `string` | `true`     | Context url. For example: `http://example.org` |
| `type`                              | `string` | `true`     | `thing` or `action` |
| `version`                           | `string` | `true`     | [Semantic Versioning](https://semver.org/) version number. For example: `1.0.1`|
| `name`                              | `string` | `true`     | Name of the ontology |
| `maintainer`                        | `string` | `true`     | Email of the maintainer |
| `classes[].class`                   | `string` | `true`     | Class (start with a capital). Noun for Things (i.e., `Animal`), verb for Action (i.e., `Moves` or `Buys`)|
| `classes[].description`             | `string` | `true`     | Description of the class |
| `classes[].keywords[].keyword`      | `array`  | `false`    | Keyword of class to better define what "keyword" of class it is. For example: `Mammal` |
| `classes[].keywords[].weight`       | `array`  | `false`    | Importance of the keyword. Minimal value = `0.0`. Maximum value = `1.0` |
| `classes[].properties[].name`       | `string` | `true`     | Name of the property (start with lowercase). For example: `name` |
| `classes[].properties[].description`| `string` | `true`     | A description of the property. For example: `Name of the animal` |
| `classes[].properties[].@dataType[]`| `array`  | `true`    | Datatype of the property. Available types are: `string`, `int`, `boolean`, `number`, `date`, `[name of class]` (for example: `Zoo`, always with capital) |
| `classes[].properties[].keywords[].keyword`         | `array`  | `false`    | Keyword of class to better define what "keyword" of property it is. For example: `identifyer` |
| `classes[].properties[].keywords[].weight`          | `array`  | `false`    | Importance of the keyword. Minimal value = `0.0`. Maximum value = `1.0` |


#### Properties

Properties describe classes by attaching values. Notes about properties:
* The value in the `@dataType` array should contain on of the following values: any uppercase starting word (like `Animal`) being a cross-reference, `string`, `int`, `number`, `boolean` or `date`.
* Every property with a certain name across all schema's should contain either a cross-reference or one of the values. For example, in this example the property `inZoo` in another class can __only__ contain a data type starting with a capital (like `Zoo`).
* Different data types can not be combined in the array.
* Weaviate validates the schemas when initializing, giving a detailed error when some of the requirements mentioned above are not met.

#### Example

Example ontology for 'Things' in a Zoo:

```json
{
	"@context": "http://zoo.example.org",
	"version": "1.0.0",
	"type": "thing",
	"name": "example.org - Thing",
	"maintainer": "hello@creativesoftwarefdn.org",
	"classes": [{
		"class": "Animal",
		"description": "A living thing other than a human being",
		"keywords": [{
			"keyword": "Beastlike",
			"weight": 0.9
		}, {
			"keyword": "Mammal",
			"weight": 0.7
		}], 
		"properties": [{
			"name": "name",
			"@dataType": [
				"string"
			],
			"description": "Name of the animal",
			"keywords": [{
				"keyword": "identifyer",
				"weight": 0.9
			}]
		}, {
			"name": "birthdate",
			"@dataType": [
				"date"
			],
			"description": "Birthdate of the animal, in ISO 8601 date format"
		}, {
			"name": "hasFeathers",
			"@dataType": [
				"boolean"
			],
			"description": "Boolean values indicating whether the animal has feathers"
		}, {
			"name": "age",
			"@dataType": [
				"number"
			],
			"description": "Age of the animal as a floating point number"
		}, {
			"name": "numberOfLegs",
			"@dataType": [
				"int"
			],
			"description": "Integer value for the number of legs the animal has."
		}, {
			"name": "inZoo",
			"@dataType": [
				"Zoo"
			],
			"description": "Location of the animal, contains a reference to a Zoo"
		}]
	}, {
		"class": "Zoo",
		"description": "parklike area in where animals are housed for exhibition",
		"keywords": [{
			"keyword": "Menagerie",
			"weight": 0.9
		}, {
			"keyword": "WildlifePark",
			"weight": 0.85
		}], 
		"properties": [{
			"name": "name",
			"@dataType": [
				"string"
			],
			"description": "Name of the zoo",
			"keywords": [{
				"keyword": "identifyer",
				"weight": 0.9
			}]
		}]
	}]
}
```

_Note:_<br>
Both [CamelCase](https://en.wikipedia.org/wiki/Camel_case) and camelCase are interpreted as being two words. Snake_case will be interpreted as one word and most probably will not be available in the vector space.

_Note II:_<br>
Although there is no distinction being made in the vector space between uppercase and lowercase. It is advised to keep classes CamelCase (or start with capital) and properties camelCase (start with lower).


## Design tips

### Schema structure

- Keep the distinction between a class and its name in mind. Classes don’t represent the words of the concepts in your domain, but the concepts itself. Therefore, synonyms for the same concept represent the same class.
- Sometimes it’s hard to decide whether a specific distinction should be modeled as property value or as a new class. Some tips:
	- If a concept with different property values becomes a restriction for properties in other classes, then the concepts should be a new class.
	- If a distinction is important in the domain, then create a new class.
	- Classes of individual instances don't change often, so if the concept you have in mind is rather static, then create a new class for it.


### Naming

- Class name labelling: use CamelCase (an uppercase working as delimiter) notation and the use of the singular form of nouns. e.g. AnimalSpecies.
- Property name labelling: property labels should always start with lowercase, but should still use uppercase word delimitation. e.g. inZoo. Property names referring to classes often need prefixes and suffixes. Common practices are to have ‘has’ as a prefix or to have the suffix ‘of’, e.g. hasFeathers and childOf.
- Avoid the use of homonyms and conjunctions.
- Recycle strings rather than using synonyms.
- Use the singular form for nouns or nominals.
- Use explicit and concise names.
- Avoid character formation (use plain <abbr>ASCII</abbr> format, avoid accents).
- Expand abbreviations and acronyms.
- Use a minimum amount of words for labels.
- Include a short but complete description of the class or property, of your own or taken from an official dictionary (e.g. from [http://www.dictionary.com/](http://www.dictionary.com/)).