---
title: Umbraco.Helpers overview
description: Overview of all the @Umbraco helpers available
---

##[Umbraco.Field](Umbraco.field)
Fetches a field from the page currently being redered. Used primarily in templates
Umbraco.Field comes with several options for modifying the output of the field
System properties such as name, creation date and others are prefixed with a @ character

##[Umbraco.GetContentById](Umbraco.GetContentById)
Gets a Content object by the given id

##[Umbraco.GetDictionaryItem](Umbraco.GetDictionaryItem)
Returns a dictionary item value, based on the given alias and current page Culture

##[Umbraco.GetDictionaryItemForLanguage](Umbraco.GetDictionaryItemForLanguage)
Returns a dictionary item value, based on the given alias and given culture code

##[Umbraco.GetDynamicContentById](Umbraco.GetDynamicContentById)
Returns a Dynamic object by the given id

##[Umbraco.GetEntityById](Umbraco.GetEntityById)
Gets the typed entity by the given id

##[Umbraco.GetMediaUrl](Umbraco.GetMediaUrl)
Gets the URL to a specifi media item, stored on a given entity

##[Umbraco.GetPrevalueModel](Umbraco.GetPrevalueModel)
Gets the pre value model for the datatype with the specified id.

##[Umbraco.GetUrl](Umbraco.GetUrl)
Gets the URL for the given entity

##[Umbraco.RenderMacro](Umbraco.RenderMacro)
Renders the macro with the specified alias, passing in the specified parameters.

##[Umbraco.Truncate](Umbraco.Truncate)
Truncates a string from a property on a dynamic object, like DynamicModel.BodyText




