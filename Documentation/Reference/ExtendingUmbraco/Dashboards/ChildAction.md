#ChildAction Dashboard
A ChildAction dashboard, consists of 2 parts. The controller, handling the logic of setting the model, and selecting the proper partial view.
This reference goes through setting up a simple Controller, a partial view, and the corresponding configuration.

##Setting up a dashboard editor controller
Setup a new class in your Controllers folder, make the class enherite from DashboardEditorController, and decorate it with a Editor attribute like so:

	using System.Web;
	using System.Web.Mvc;
	using Umbraco.Cms.Web.Editors;
	using Umbraco.Cms.Web.Context;
	
	
	namespace My.Controllers.Dashboards
	{
	    //Decorate class with a Editor attribute
	    [Editor("515C988C-0160-4A21-9432-884C59854909", HasChildActionDashboards = true)]
	    public class HelloWorldDashboardController : DashboardEditorController
	    {
	        //Default constructor for DashboardEditorControllers
	        public HelloWorldDashboardController(IBackOfficeRequestContext requestContext)
	            : base(requestContext)
	        {
	
	        }
	    }
	}

##Adding a PartialViewResult action
Add a method, which returns a PartialViewResult to the HelloWorldDashboardController class.
Perform whatever logic you want in the action, make sure, to mark the action as [ChildActionOnly](http://msdn.microsoft.com/en-us/library/system.web.mvc.childactiononlyattribute.aspx)


	[ChildActionOnly]
	public PartialViewResult SaySomething()
	{
		ViewBag.Saying = "Hello there";
		return PartialView("HelloWorldView");
	}

The action, returns a PartialView, so in this case, umbraco will start searching for a cshtml/vbhtml file in its view folders, these folders include your plugins viewfolders, as well as Umbracos default views folders + shared views folders. The exemple above looks for a file with the name `HelloWorldView.cshtml`

	

##Setting up a dashboard partial view
Create a file in /views, named `HelloWorldView.cshtml` In the view file add the following:

	@inherits System.Web.Mvc.WebViewPage
	@using Umbraco.Cms.Web;

	<h1>I say: @ViewBag.Saying</h1>

Save the file.


##Configuration
For this to be displayed in the umbraco backoffice, you need to add a setting the umbraco dashboards config or add it as part of a plugin. For this we will edit the umbraco dashboards file [More about plugins](../plugins/index.md)

Open the file `/app_data/umbraco/config/umbraco.cms.dashboards.config` find the `<dashboards>`element and add the below element as a child of that.

	<add tab="Hello world" type="childAction" name="HelloWorldDashboard.SaySomething" />

Copy the dll from your project to the /bin, copy the HelloWorldView.cshtml view to the /views/shared folder and open /umbraco/. Your dashbord should now be displayed.


##Follow dashboard UI conventions
To make your UI component look and feel like a native dashboard, follow the markup conventions. So enhance the HelloWorldView view file like so:

	@inherits System.Web.Mvc.WebViewPage
	@using Umbraco.Cms.Web;
	
	<h3 class="property-pane-header">@ViewBag.Saying</h3>
	<div class="property-pane" id="dashboardCreate">
		<div>
			<p>this is my pane</p>
		</div>
		
		<div class="property-editor clearfix">
			<div class="property-editor-label" title="Name">
			    Name
			</div>
			<div class="property-editor-control">
			    <input name="Name" id="Name" data-val="true" data-val-required="The Name field is required." type="text"/>
			    <span class="field-validation-valid" data-valmsg-for="Name" data-valmsg-replace="true"></span>
			</div>
		</div>
	</div>
	
