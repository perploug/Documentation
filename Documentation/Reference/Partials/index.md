---
title: Partials
description: Overview of the items related to partials
---

#Partials
A partial in Umbraco, is an insolated piece of functionality / markup, you
wish to keep seperate from your ordinary templates.
Either because you wish to reuse it, or to keep your templates clear of any programming
logic.

A partial is by default written in [Razor](razor), which is the scripting language Umbraco (and Asp.net MVC 3)
uses for templating.

##Creating a Hello World partial
Go to the settings section, right-click the "partials" folder, choose create, and enter a name for your partial.

By default the partial contains the below:

	@inherits RenderViewPage
	@using Umbraco.Cms.Web;

Append the following to the partial and save it:

	<h1>Hello @DynamicModel.Name</h1>
	
Now, open one of your templates, and click the "insert partial" button on the toolbar, and choose your partial from the dropdown list. This inserts the following in your **template**:

	@Html.Partial("TheAliasOfYourPartial")

Open a page, using this template in your browser, and a `<H1>`  element will be rendered.

##Edit in visual studio or webmatrix
If you wish to edit the partial from an editor outside of umbraco (you can use visual studio, webmatrix or even notepad) it will be located on the path: **/Views/Umbraco/Partial/AliasOfthePartials.cshtml**

Visual Studio and Webmatrix 2 provides helpfull intellisense duting editing

-----

#Samples
##[Hello world](HelloWorld)
A simple hello world sample to get you started with umbraco partials

##[Simple Navigation](Navigation)
A walkthrough on how to create a simple navigation

##[Gallery](Gallery)
A walkthrough on how to create a simple gallery

----

#Reference
##[RenderViewPage](RenderViewPage)
The View that front-end templates inherit from, containing references to all the
core items needed to render Umbraco data to your views

##[DynamicModel](DynamicModel)
Partials uses DynamicModel for accesing the current page being rendered, as well as for querying, sorting and getting data from the umbraco datastore (hive), and has a collectiong of methods for making this as straight-forward as possible.

##[Razor](razor)
A syntax overview of the language used in Partials.  

