# Document Type

## What is a Document Type?
A Document Type is a model / data definition for your content nodes. Every content node on an Umbraco web site always maps to a backing Document Type.

#### Composition
A Document Type is composited by Properties, which are grouped by Tabs. It can also inherit properties and tabs from other Document Types.

It is also possible to link one or more Templates to a Document Type to choose how you want your model / data rendered to the user.

#### Properties
A Property is used to store a single piece of information for a content node. Every Property consists of the following information:

- Name
	- The display name of the property that will be show to the user when creating/editing a node in the Content section.
- Alias
	- The "key" for the property.
- [Data Type](https://github.com/umbraco/Umbraco5Docs/blob/5.0.1/Documentation/Concepts/BackOffice/Property-Editors.md)
	- The Property Editor that will be used to store values for the Property.
- Tab
	- Which tab the property should be placed in.
- Description
	- A description of the property that will be show to the user when creating/editing a node in the Content section.