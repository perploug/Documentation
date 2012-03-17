#Umbraco.RenderMacro
Renders the macro with the specified alias, passing in the specified parameters.

##Samples
**Render a macro with the alias list**

	@Umbraco.RenderMacro("list")

**Render a macro with the alias ContactForm, and pass the parameters "receiver", and "subject"**

	@Umbraco.RenderMacro("ContactForm", 
		new { Receiver = "per@mail.dk", 
		subject = "Hello" })

##Parameters
###Macro alias
`string` case-sensitive alias of the macro

###Macro Parameters
`object` Anonymous object defining parameters passed to the macro following the below syntax:

	new { 
		parameterA = "per@mail.dk", 
		paramB = "Hello" 
	}