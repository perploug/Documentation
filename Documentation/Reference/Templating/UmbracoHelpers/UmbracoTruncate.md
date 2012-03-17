#Umbraco.Truncate
Truncates a string if longer then a given length. Can optionally suffix the string with a given value. 

##Samples

	@{
		//truncate bodyText if longer 300 chars, and suffix it with "..."
		var truncatedString = Umbraco.Truncate(DynamicModel.bodyText, 300, "...");
	}

##Parameters
###Value
`string` The string to truncate

###Length
`int` The maximum length

###_Suffix_
`string` String to suffix truncated value with
