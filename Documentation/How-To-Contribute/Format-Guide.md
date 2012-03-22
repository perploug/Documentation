#Format Guide

##Writing a getting started guide

The getting started guides are considered as tutorials and are therefor written as step by step guides with accompanying screenshots.

A getting started guide should always start with a short and concise summary on the subject of the guide, so the reader will get an understanding of what the current document is about, who the targeted audience is and what the key takeaways are.

If any specific software is required to follow along a getting started guide we consider it good practice to add a Prerequisites section to the beginning of the document.

##Writing about an Umbraco concept

A concept is a description of a specific characteristic of Umbraco found in either the backoffice or API. Examples of concepts found in the backoffice could be a Document Type or a Template, while concepts found in the API could be Hive or a TypedEntity.

A document describing a concept should contain a short summary on the content of the current concept. This could be a one liner describing what a Document Type is, e.g. “A Document Type is the data definition for your content”.
The rest of the document should go into greater detail describing the concept to the reader, so he gets a better understanding of the concept and how it can be used. If you need to touch upon other concepts please link to them and try not to go into detail about other concepts as they are described in separate documents.
Snippets for the cheat sheet section

A snippet is a piece of usable code (no pseudo code), which shows a specific type of functionality like how to create a navigation using razor or how to get and display an image using the umbraco helpers.
A snippet should always have an introduction explaining what the snippet does and maybe even list an example of the expected output. If any special parameters are used these should be explained as well.
The supplied code snippet should be thoroughly commented, so the reader can follow along and better understand what the different parts of the code does.

##API References

API references are usually very technical in nature, but should at least have a simple introduction about the topic, which the current reference covers.
If needed write a short section about getting started, which could include which namespace(s) the reference will cover and where it can be used, e.g. in Views, Partials or Controllers.

The main part of an API reference will be a look through of the Properties and Public Methods, which are available through this part of the API.

##Writing about a best practice

An article about a best practice can be written in many ways, and it should really be up to the writer what it should contain. So the only format to consider for the overall outline of the article is that it should contain a short introduction explaining what the current article is about to give the reader an idea of what this best practice is covering.
Writing a document for the “Help for designers/developers/editors/system administrators”-section

Articles for these four sections can vary in form being either “getting started”-types of documentation, tutorials about creating something like a dashboard (or anything else in Umbraco for that matter) or cheat sheets to help a designer and/or developer create i.e. a certain type of macro. So the only format to follow is to have an introduction and then the main article.
Please make sure to link out to concepts or references whenever you touch upon these in your article.

##Entries for the glossary

The glossary is a quick reference for anything and everything referenced in Umbraco, which could be the technologies and libraries used, concepts or main parts of the API.
An entry in the glossary is simply one line of text with links to relevant parts in the documentation or external links.