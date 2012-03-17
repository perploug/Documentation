#RenderViewPage
RenderViewPage is the class inherited by all Partials. It contains references to the current page, access to Hive for queries and the RequestContext. `RenderViewPage` inherits from the standard MVC class `System.Web.Mvc.WebViewPage`
	
##Usage
Use `RenderViewPage` to access the page, hive or Request context in your Partials or templates. Both templates and partials inherit from this class, and the syntax is the same, as both partials and templates are based on Razor.

	@inherits RenderViewPage
	@using Umbraco.Cms.Web;
	@using Umbraco.Hive.RepositoryTypes;
		
	@{
	    //get a hive writer from Hive
	    var hiveWriter = Hive.GetWriter<IContentStore>();
	
	    //Get the path to the backoffice with the RequestContext
	    var backofficeFolder = RoutableRequestContext.Application.Settings.UmbracoFolders.BackOfficeFolder;
	
	    //get url from from currentPage from Umbraco helper
	    var url = Umbraco.GetUrl(DynamicModel);
	
	    //get field from current page
	    var field = Umbraco.Field("bodyText");
	}

##Properties
###RoutableRequestContext
Encapsulates information specific to a request handled by Umbraco, including the Umbraco application context which contains services which last for the lifetime of the application, registered plugins, and the routing engine used.

###Hive
Something to do with data, not really sure what this does... 

###[Umbraco](Umbraco-Helpers/index.md)
A utility class for use with front-end development containing many methods for accessing common APIs in Umbraco

##Properties inherited from WebViewPage
As RenderViewPage inherites from WebViewPage, it also exposes the following properties. For complete documentation on the Mvc WebViewPage, to to the [MSDN documentation](http://msdn.microsoft.com/en-us/library/system.web.mvc.webviewpage_properties(v=vs.98).aspx)

###Ajax	
Gets or sets the AjaxHelper object that is used to render HTML using Ajax.

###Context	
Gets the HttpContext object that is associated with the page. (Overrides WebPageExecutingBase.Context.)

###Culture	
Gets or sets the culture for the current thread

###Html	
Gets or sets the HtmlHelper object that is used to render HTML elements.

###IsAjax	 
Get a value that indicates whether Ajax is being used during the request of the web page.

###IsPost	 
Returns a value that indicates whether the HTTP data transfer method used by the client to request the web page is a POST request.

###Layout	 
Gets or sets the path of a layout page

###Model	
Gets the Model property of the associated ViewDataDictionary object.

###Output	 
Gets the current TextWriter object for the page.

###OutputStack	 
Gets the stack of TextWriter objects for the current page context.

###Page	
Provides property-like access to page data that is shared between pages, layout pages, and partial pages.

###PageContext	 
Gets the HTTP context for the web page.

###PageData	 
Provides array-like access to page data that is shared between pages, layout pages, and partial pages.

###Request	 
Gets the HttpRequest object for the current HTTP request.

###Response	 
Gets the HttpResponse object for the current HTTP response.

###Server	 
Gets the HttpServerUtility object that provides methods that can be used as part of web-page processing.

###Session	
Gets the HttpSessionState object for the current HTTP request.

###TempData	
Gets the temporary data to pass to the view.

###TemplateInfo	 
Gets information about the currently executing file.

###UICulture	 
Gets or sets the current culture used by the Resource Manager to look up culture-specific resources at run time.

###Url	
Gets or sets the URL of the rendered page.

###UrlData	
 Gets data related to the URL path.

###User	 
Gets a user value based on the HTTP context.

###ViewBag	
Gets the view bag.

###ViewContext	
Gets or sets the information that is used to render the view.

###ViewData	
Gets or sets a dictionary that contains data to pass between the controller and the view.