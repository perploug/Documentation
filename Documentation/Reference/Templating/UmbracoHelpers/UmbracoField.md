#Umbraco.Field
Fetches a field from the page currently being rendered. Used primarily in templates and partials.

Umbraco.Field comes with several options for modifying the output of the field
System properties such as name, creation date and others are prefixed with a @ character

**Notice** As of version 5.0, Umbraco.Field is the only way to resolve internal links and macros, inseted in Rich Text Editors, you therefore have to use Umbraco.Field to render a RTE value correctly, this is bound to change in 5.0.1

##Samples
	//get the page name
	@Umbraco.Field("@name");

	//get the field with the alias "bodyText" from a given content object
	@Umbraco.Field(DynamicModel.Parent, "bodyText")
	
	//Use all the paramters you wish, they mix and match as you want them to
	@Umbraco.Field("bodyText", 
		altFieldAlias: "siteName", 
		altText: "Not Found", 
		recursive: true, 
		insertBefore: "&lt;h1&gt;", 
		insertAfter: "&lt;/h1&gt;", 
		casing: global::Umbraco.Cms.Web.UmbracoRenderItemCaseType.Upper, 
		encoding: global::Umbraco.Cms.Web.UmbracoRenderItemEncodingType.Html, 
		convertLineBreaks: true, 
		removeParagraphTags: true, 
		formatString: "d")




##Available Parameters
###fieldAlias
`string`, Alias of the field you want to insert

	//get the field with the alias "bodyText"
	@Umbraco.Field("bodyText")

###_currentPage_
`Content`, the content object you wish to retrieve the field value from. If non given, use current page

	//get the field with the alias "bodyText" from a given content object
	@Umbraco.Field(DynamicModel.Parent, "bodyText")


###_altFieldAlias_
`string`, if the fieldAlias is not found on the current page, try inserting the alt field value instead
	//if bodyText does not exists, insert value from numberOfpages
	@Umbraco.Field("bodyText", altFieldAlias: "numberOfPages")

###_altValueAlias_
`string`, only used with altFieldAlias, where a complex value is returned. The value alias, describes which item from the key/value field should be inserted. Usually used with Upload fields, which contains multiple values

	//if bodytext does not exists, insert the MediaId value from the image field
	@Umbraco.Field("bodyText", altFieldAlias: "image", altValueAlias: "MediaId")

	
###_altText_
`string`, define a standard value, in cases the selected field is not found

	//if bodytext is not found on the current page
	@Umbraco.Field("bodyText", altText: "Not found")
	
	//if bodyText nor altBody is found on the current page
	@Umbraco.Field("bodyText", altFieldAlias: "altBody", altText: "Neither is found")
	
	
###_casing_
`UmbracoRenderItemCaseType`, Expects a Casing type, to define what casing should be used, possible values are: 

- `global::Umbraco.Cms.Web.UmbracoRenderItemCaseType.Upper`
- `global::Umbraco.Cms.Web.UmbracoRenderItemCaseType.Lower`
- `global::Umbraco.Cms.Web.UmbracoRenderItemCaseType.Title`

	//Convert the field value to Upper case
	@Umbraco.Field("@name", casing: global::Umbraco.Cms.Web.UmbracoRenderItemCaseType.Upper)

	
###_convertLineBreaks_
`true/false`, toggle whether line breaks in strings should be converted to `<p>` tags 

	@Umbraco.Field("sectionDesc", convertLineBreaks: true)


###_encoding_
`UmbracoRenderItemEncodingType`, Expects a Encoding type, to define what encoding should be used, possible values are: 

- `global::Umbraco.Cms.Web.UmbracoRenderItemEncodingType.Html`
- `global::Umbraco.Cms.Web.UmbracoRenderItemEncodingType.Url`

	//Html Encoding value
	@Umbraco.Field("@name", encoding: global::Umbraco.Cms.Web.UmbracoRenderItemEncodingType.Html)
	
	//Url Encoding value
	@Umbraco.Field("@name", encoding: global::Umbraco.Cms.Web.UmbracoRenderItemEncodingType.Url)
	

###_formatString_
`string`, Formats values, folllowing the C# format syntax, can be used on [numeric](http://msdn.microsoft.com/en-us/library/dwhawy9k.aspx) and [DateTime](http://msdn.microsoft.com/en-us/library/az4se3k1.aspx) values

	//Format createDate as a short date
	@Umbraco.Field("@createDate", formatString: "d")
	
	//Format createDate as a short date
	@Umbraco.Field("@createDate", formatString: "d")
	
	//custom format
	@Umbraco.Field("@createDate", formatString: "m/d/yyyy")

	
	
###_insertbefore_
`string`, value to insert before the field value, will only be rendered if a value is found, values must be html encoded

	//wrap SectionName value in a <h1> tag
	@Umbraco.Field("SectionName", insertBefore: "&lt;h1&gt;",  insertAfter: "&lt;/h1&gt;", )


###_insertAfter_
`string`, value to insert after the field value, will only be rendered if a value is found, values must be html encoded

	//wrap SectionName value in a <h1> tag
	@Umbraco.Field("SectionName"insertBefore: "&lt;h1&gt;", insertAfter: "&lt;/h1&gt;", )
	

###_recursive_
`true/false`, indicate if umbraco should retrieve value from parent pages

	//Get the field sectionName recursively
	@Umbraco.Field("sectionName", recursive: true)


###_removeParagraphTags_
`true/false` toggle, whether `<p>` should be stripped from the value

		@Umbraco.Field("sectionDesc", removeParagraphTags: true)

