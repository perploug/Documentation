#DynamicModel
DynamicModel is the dynamic access to all the data stored in your Umbraco website. Also know as the Model of your site.
DynamicModel represents the current page, curently being rendered, and is usually referenced on Templates, Partials or Macros

DynamicModel is a dynamic object, this gives us a number of obvious benefits, mostly a much lighter syntax for 
accessing data, as well as dynamic queries, based on the names of your DocumentTypes and their properties, but
at the same time, it does not provide intellisense.


##Get started
To access the current page in your Partials, macros or templates, copy-paste the below razor code.

	@{
		var currentPage = DynamicModel;
		var pageName = DynamicModel.Name;
		var childPages = currentPage.Children;
	}
	
	<h1>@pageName</h1>

##Properties
Built-in properties, which exists on all dynamic content objects by default. These are referenced in Razor as a standard property
`object.Property` using standard c syntax. 

	@* gets the current page Url *@
	@DynamicModel.Url
	
	@* gets the Creation date, and formats it to a short date *@
	@DynamicModel.CreateDate.ToString("D")
	
	@* Outputs the name of the parent if it exists *@
	@if(DynamicModel.Parent != null){
		<h1>@DynamicModel.Parent.Name</h1>
	}

###Name
Returns the Name of the current content item

###Id
Returns the unique `HiveId` for the current content item

----

###.CreateDate
Returns the DateTime the page was created

###.UpdateDate
Returns the DateTime the page was modified

###.StatusChangedData
Returns the DateTime the status of the page was changed

-----

###.Url
Returns the complete Url to the page

###.NiceUrl
Same as Url

###.UrlName
Returns Url encoded name of the page

------

###.NodeTypeAlias
Returns the Alias of the Document type used by this content item.

###.Template
Returns the Template object used by this content item.

###.TemplateId
Returns the ID of the current template

###.TemplateFileName
Returns the filename of the current template

------

###.Parent
Returns a dynamic object, referencing the parent of the current page. If current page is at the top of the site tree, null will be returned

###.Path
Returns the Hive `EntityPath` to this content item

###.PathById
Returns the Hive `EntityPath` to this content item

###.Level
Returns the Level this content item is on

###.SortOrder
Returns the index the page is on, compared to its siblings

###.__OriginalItem
References the actual `Content` object which the dynamic object is based on

----

##Custom properties
All content and media items also contains a reference to all the data defined by their document type
	
###DynamicModel.PropertyAlias
Returns the property matching the PropertyAlias (replace with alias of property) 

	@*Get the property with alias: "siteName" from the current page  *@
	@DynamicModel.siteName
	
	@*Notice razor uses Pascal casing, there is therefore an OverLoad to get propeties as pascal cased as well*@
	@DynamicModel.SiteName
	
	@*Or use Umbraco.Field *"
	@Umbraco.Field("siteName");
	
	
	
	@*Get the property with alias "bodyText" from the parent page*@
	@DynamicModel.Parent.bodyText
	
	@*Notice, to render RTE content, and resolve links, this should be done with Umbraco.Field *@
	@Umbraco.Field(DynamicModel.Parent, "bodyText")
	

**Notice**, that Razor uses Pascal casing (capitalize first letter) for properties.
Propety `bodyText` can therefore be referenced as `BodyText`


###DynamicModel._propertyAlias
Returns the property matching the propertyAlias (replace with alias of property) 
by prefixing with '_' razor will first look on the current page. If no value is defined, it will then search ancestor pages for a property matching the alias, and return a value, if a property is found.

	@* Get the "siteName" property recursively (if not present on current page, traverse 
	through page ancestors, 
	Notice this matches alias casing, but prefixes a _ char *@
	@DynamicModel._siteName
	
	@*OR, use Umbraco.Field*@
	@Umbraco.Field("bodyText", recursive:True)

**Notice**, this matches the exact casing for the property.
Propety `bodyText` is therefore referenced as `_bodyText`

----

##Collections
All collections returned by DynamicModel is of the type DynamicContentList, which has a collection of properties themselves.

###.[DocumentTypeAlias]s (Pluralised Collections)
Returns the children of the current page, matching the Document type alias

**Get all children of type with alias: 'textPage'**

	@var collection = DynamicModel.TextPages
	
**Get the first child page of type with alias: 'homePage'**

	@var page = DynamicModel.HomePages.First()


###.Children
Returns a collection of items just below the current content item

	<ul>
		@foreach(var item in DynamicModel.Children){
			<li><a href="@item.Url">@item.Name</a></li>
		}
	</ul>


###.Ancestors
Returns all ancestors of the current page (parent page, grandparent and so on)

	<ul>
		@*Order items by their Level*@
		@foreach(var item in DynamicModel.Ancestors.OrderBy("Level")){
			<li><a href="@item.Url">@item.Name</a></li>
		}
	</ul>

###.AncestorsOrSelf
Returns all ancestors of the current page (parent page, grandparent and so on), and the current page itself

	@* Get the top item in the content tree, this will always be the Last ancestor found *@
	var websiteRoot = DynamicModel.AncestorsOrSelf.Last();


###.Descendants
Returns all descendants of the current page (children, grandchildren etc)

	<ul>
		@*Filter collection by the document type alias*@
		@foreach(var item in DynamicModel.Descendants.Where("NodeTypeAlias = @0", "newsItem")){
			<li><a href="@item.Url">@item.Name</a></li>
		}
	</ul>

###.DescendantsOrSelf
Returns all descendants of the current page (children, grandchildren etc), and the current page itself
Returns all descendants of the current page (children, grandchildren etc)

	<ul>
		@*Filter collection by the document type alias*@
		@foreach(var item in DynamicModel.Descendants.Where("NodeTypeAlias = @0", "newsItem")){
			<li><a href="@item.Url">@item.Name</a></li>
		}
	</ul>

##HiveID collections
Besides the regular collections of dynamic Content. DynamicModel also has collections of the Raw Hive Ids, which can be used for faster ID lookups

###.AllAncestorIds
Returns `Array[HiveID]` containing only the ids of ancestor pages, of the current page

	<ul>
		@foreach (var item in Homepage.Children)
		{
			var cssClass = Array.IndexOf(DynamicModel.AllAncestorIds, item.Id) >= 0 ? "current" : "notCurrent";
			<li><a class="@cssClass" href="@item.Url">@item.Name </a></li>
		}
	</ul>

###.AllAncestorIdsOrSelf
Returns `Array[HiveID]` containing only the ids of ancestor pages, of the current page, including the ID of the current page

	<ul>
		@foreach (var item in Homepage.Children)
		{
			var cssClass = Array.IndexOf(DynamicModel.AllAncestorIdsOrSelf, item.Id) >= 0 ? "current" : "notCurrent";
			<li><a class="@cssClass" href="@item.Url">@item.Name </a></li>
		}
	</ul>

###.AllDescendantIds
Returns `Array[HiveID]` containing only the ids of descendant pages, of the current page

	var id = DynamicModel.ContentPickerProperty;
	
	var exists = Array.IndexOf(DynamicModel.AllAncestorIdsOrSelf, id) >= 0;

###.AllDescendantIdsOrSelf
Returns `Array[HiveID]` containing only the ids of descendant pages, of the current page, including the ID of the current page

	var id = DynamicModel.ContentPickerProperty;
	
	var exists = Array.IndexOf(DynamicModel.AllAncestorIdsOrSelf, id) >= 0;

###.AllChildIds
Returns `Array[HiveID]` containing only the ids of descendant pages, of the current page

	var id = DynamicModel.ContentPickerProperty;
	
	var exists = Array.IndexOf(DynamicModel.AllAncestorIdsOrSelf, id) >= 0;

##Traversing
###.First(optional "condition")
Returns the first item, matching the given condition, if no match, an exception is thrown

	@var NewsArea = DynamicModel.NewsAreas.First();
	
	@var NewsAreaWithTopic = DynamicModel.NewsAreas.First("Topic == @0", "cars");
	

###.FirstOrDefault(optional "condition")
Returns the first item, matching the given condition, if no match, null is returned

	@var NewsArea = DynamicModel.NewsAreas.FirstOrDefault();
	
	@var NewsAreaWithTopic = DynamicModel.NewsAreas.FirstOrDefault("Topic == @0", "cars");


###.ElementAt(index)
Returns the item at a given index in the collection, if index is out of bounds, an exception is thrown
	
	@var thirdPage = DynamicModel.Children.ElementAt(2);
	
###.ElementAtOrDefault(index)
Returns the  item at a given index in the collection, if index is out of bounds, null is returned

	@var thirdPage = DynamicModel.Children.ElementAt(2);


###.Last(optional "condition")
Returns the last item, matching the given condition, if no match, an exception is thrown

	@var root = DynamicModel.AncestorsOrSelf.Last();
	
	@var almostRoot = DynamicModel.Ancestors.Last("NodeTypeAlias == @0", "newsArea");
	
###.LastOrDefault(optional "condition")
Returns the last item, matching the given condition, if no match, null is returned

	@var root = DynamicModel.AncestorsOrSelf.AncestorsOrSelf();
	
	@var almostRoot = DynamicModel.Ancestors.AncestorsOrSelf("NodeTypeAlias == @0", "newsArea");

###.Single(optional "condition")
Returns any item, matching the given condition, if no match, an exception is thrown

	@var child = DynamicModel.Children.Single();
	
	@var childWithSectionName = DynamicModel.NewsItems.Single("SectionName == @0", "cars");
	
	
###.SingleOrDefault(optional "condition")
Returns the any item, matching the given condition, if no match, null is returned

	@var child = DynamicModel.Children.SingleOrDefault();
	
	@var childWithSectionName = DynamicModel.NewsItems.SingleOrDefault("SectionName == @0", "cars");

##Filtering, Ordering & Expressions
###.And("condition")
Part of a filtering chain, and can only be used together with .Where()

	@var Toyotas = DynamicModel.Children.Where("Section == @0", "cars").And("Model == @0", "toyota");
	
	@var theMazda =  DynamicModel.Children.Where("Section == @0", "cars").And("Model == @0", "mazda").First();

###.Any()
Returns True if the collection contains any items at all, otherwise False
	
	@bool hasChildren = DynamicModel.Children.Any();
	
###.Count()
Returns the number of items in the collection

	@int numberOfChildren =  DynamicModel.Children.Count();
	
###.OrderBy("fieldname")
Orders a collection by a field name ascendingly 
	
	//order by name
	@var nodes = DynamicModel.Children.OrderBy("Name");
	
	//Order by custom property
	@var nodes = DynamicModel.Children.OrderBy("SectionName");
	
###.OrderByDescending("fieldname")
Orders a collection by a field name descendingly 

	//order by name
	@var nodes = DynamicModel.Children.OrderByDescending("Name");
	
	//Order by custom property
	@var nodes = DynamicModel.Children.OrderByDescending("SectionName");
	
###.ToList()
Returns a `List<dynamic>` 
		

###.IndexOf(item)
Returns the index of a given item in the collection, if not present, it returns -1

	//get random item
	@var item = DynamicModel.Children.Single();
	
	//get the index
	@var indexOfitem = DynamicModel.Children.IndexOf(item);

###.Where("condition")
Returns all items matching the given condition.
For more details on queries and conditions, see the chapter below

	@var ancestors = DynamicModel.Where("UmbracoNaviHide != @0", "True");
	
	@var HomePage = DynamicModel.Where("NodeTypeAlias == @0 and Level == @1 or Name = @2", "HomePage", 0, "Home").First();


##Conditions for filtering and traversing
When using dynamic model for filtering or traversing data, you have the option of setting a condition that most be true for the item to pass a filter or be selected. These conditions follow their own syntax, which is somewhat like SQL. 

The ExpressionParser is however very limited, due to the nature of dynamic objects. The current version only supports a subset of the operators you would normally have available in c#.

Below are a couple of exemples of these:

###Condition structure

	.Method(string Condition, object[] substitutes)
	
###Placeholders

	//select items where the Document Type is Homepage
	//Notice the replacement syntax
	.Where("NodeTypeAlias == @0", "homePage")
	
	//multiple replacements
	.Where("NodeTypeAlias == @0 and Level == @1", "homePage", 1) 
	
###Operators

####==

	.Where("NodeTypeAlias == @0", "homePage")

####!=

	.Where("UmbracoNaviHide != @0", "True")

####and / &&

	.Where("UmbracoNaviHide != @0 and NodeTypeAlias == @1", "True", "HomePage")

####or / ||

	.Where("UmbracoNaviHide == @0 or NodeTypeAlias == @1", "True", "HiddenPage")
