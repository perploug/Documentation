#Templating
Templating consists of several concepts, but all have some things in common: the razor language syntax, access to DynamicModel, and the use of the Umbraco.Helpers library. 

##[Working with partials](partials/index.md)
Partials are used for lists, navigation and other seperate chunks of functionality and markup.


##[Working with macros](MacroPartials/index.md)
Macros expands on the concept of a parial view, and enables you to pass parameters from
your pages to your macor partial views, making them more flexible.


##Working with templates
**coming soon**, Templates can be devided into sections, inherite from other templates, and can contain partials and macros. 


##References
###[DynamicModel](DynamicModel.md)
Partials and templates uses DynamicModel for accesing the current page being rendered, as well as for querying, sorting and getting data from the umbraco datastore (hive), and has a collectiong of methods for making this as straight-forward as possible.

###[Razor](razor/index.md)
A syntax overview of the language used in Partials and templates.

###[Umbraco Helpers](UmbracoHelpers/index.md)
A utility class for use with front-end development containing many methods for accessing common APIs in Umbraco

###[RenderViewPage](RenderViewPage.md)
The View that front-end templates inherit from, containing references to all the
core items needed to render Umbraco data to your views

###Client Dependency
Coming soon