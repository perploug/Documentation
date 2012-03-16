#Umbraco.Helpers
A utility class for use with front-end development containing many methods for accessing common APIs in Umbraco


##[Umbraco.Field](Umbracofield.md)
Fetches a field from the page currently being redered. Used primarily in templates
Umbraco.Field comes with several options for modifying the output of the field
System properties such as name, creation date and others are prefixed with a @ character

##[Umbraco.GetContentById](UmbracoGetContentById.md)
Gets a Content object by the given id

##[Umbraco.GetDictionaryItem](UmbracoGetDictionaryItem.md)
Returns a dictionary item value, based on the given alias and current page Culture

##[Umbraco.GetDictionaryItemForLanguage](UmbracoGetDictionaryItemForLanguage.md)
Returns a dictionary item value, based on the given alias and given culture code

##[Umbraco.GetDynamicContentById](UmbracoGetDynamicContentById.md)
Returns a Dynamic object by the given id

##[Umbraco.GetEntityById](UmbracoGetEntityById.md)
Gets the typed entity by the given id

##[Umbraco.GetMediaUrl](UmbracoGetMediaUrl.md)
Gets the URL to a specifi media item, stored on a given entity

##[Umbraco.GetPrevalueModel](UmbracoGetPrevalueModel.md)
Gets the pre value model for the datatype with the specified id.

##[Umbraco.GetUrl](UmbracoGetUrl.md)
Gets the URL for the given entity

##[Umbraco.RenderMacro](UmbracoRenderMacro.md)
Renders the macro with the specified alias, passing in the specified parameters.

##[Umbraco.Truncate](UmbracoTruncate.md)
Truncates a string if longer then a given length. Can optionally suffix the string with a given value. 