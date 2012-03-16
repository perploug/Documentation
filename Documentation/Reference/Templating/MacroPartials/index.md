#MacroPartials
A MacroPartial in Umbraco, is a wrapper around a normal MVC [partial view](../Partials/index.md), which allows
you to pass umbraco specific paramters to the partial at runtime.

A MacroPartial consists of 2 parts:
* a .cshtml/.vbhtml file
* a macro object defining possible parameters

##Creating a Hello World macro partial
Go to the developer section, right-click the "macro partials" folder, choose create, and enter a name for your macro partial, notice that "create macro" is checked.

By default the macro partial contains the below:

	﻿@inherits PartialViewMacroPage
	@using Umbraco.Cms.Web
	@using Umbraco.Cms.Web.Macros
	@using Umbraco.Framework

Append the following to the partial and save it:

	<h1>Hello @DynamicModel.Name</h1>
	
Now, open one of your templates, and click the "insert macro" button on the toolbar, and choose your partial from the dropdown list. This inserts the following in your **template**:

	@Umbraco.RenderMacro("nameOfYourMacro")

Open a page, using this template in your browser, and a `<H1>`  element will be rendered.

##Add a parameter to the Macro partial
Go the developer section, and open the macro in the "Macros" folder. Go to the "Macro Parameters" tab
and add a new Parameter.

Check "show", set alias to "myName", set name to "My Name" and Type to "Textstring", and save the macro.

Now open the Macro partial file in the "Macro Partials" folder, and append the below line:

	Param: @Model.MacroParameters.myName

Insert the macro on a template again. This time you will be prompted for a value for "myName" parameter, this results in the below RenderMacro

	@Umbraco.RenderMacro("macroALias", new { myName = "Per" })

When the page renders `@Model.MacroParamters.myName` gets the value "Per" set.


##Edit in visual studio or webmatrix
If you wish to edit the partial from an editor outside of umbraco (you can use visual studio, webmatrix or even notepad) it will be located on the path: **/Views/Umbraco/MacroPartial/AliasOfthePartials.cshtml**

Visual Studio and Webmatrix 2 provides helpfull intellisense duting editing

----

##Reference
###[RenderViewPage](../RenderViewPage.md)
The View that front-end templates inherit from, containing references to all the
core items needed to render Umbraco data to your views

###[DynamicModel](../DynamicModel.md)
Partials uses DynamicModel for accesing the current page being rendered, as well as for querying, sorting and getting data from the umbraco datastore (hive), and has a collectiong of methods for making this as straight-forward as possible.

###[Razor](../razor/index.md)
A syntax overview of the language used in Partials.  

-----

##[Samples](samples/index.md)
A big collection of macro partial samples are available, browse these
to see how to use the different parameter types, how to render more advanced
navigation components.