#Umbraco.GetDynamicContentById
Returns a dynamic object, given a unique ID, as either a `string` or a `HiveId`

##Samples

**get a content item based on a string** 

	@{
		//a string, representing a ID
		var id = "content://p__nhibernate/v__guid/00000000000000000000000000001048";
		dynamic item = Umbraco.GetDynamicContentById(id);
			
		//get the name of the item
		var name = item.Name;
	}

**get a HiveId from the AncestorID collection**

	@{
		//get the id from the ancestorID collection
		HiveId id = Umbraco.AllAncestorIds.Single();
		dynamic item = Umbraco.GetDynamicContentById(id);
	} 

**Return DynamicModel.Parent in case nothing is found with the give Id**

	@{
		//get the id from the ancestorID collection
		HiveId id = Umbraco.AllAncestorIds.Single();
		dynamic item = Umbraco.GetDynamicContentById(id, DynamicModel.Parent);
	} 


##Parameters

###Id
`string` or `HiveId`  The id of the content item you wish to retrieve

###_DefaultValue_
`object` Value returned in case no item with the given Id exists. by default a new `BendyObject` is returned
