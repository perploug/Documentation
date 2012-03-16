#PartialView Dashboard
PartialView-based dashboards, consists of a single cshtml/vbhtml file, and some configuration.

##Setup a Dashboard partial view file
Create a new file in /views/shared and call it "HelloWoldDashboard.cshtml"

Copy/paste in the following markup:

	@inherits Umbraco.Cms.Web.Dashboards.DashboardViewPage
	@using Umbraco.Framework
	@using Umbraco.Cms.Web
	@using ClientDependency.Core.Mvc

	@{
	    //Load Some javascript if you need to.
	    Html.RequiresJs("folder/script.js", "Scripts");
	    Html.RequiresJs(Url.GetPluginsFolder() +  "/Core/Scripts/Dashboards.js");         
	}  


	<h3 class="property-pane-header">Hai world!</h3>
	<div class="property-pane">
		<div>
			<p>Hai, I'm a custom dashboard</p>
		</div>
	</div>
	
Save the file.

##Configuration
For this to be displayed in the umbraco backoffice, you need to add a setting the umbraco dashboards config or add it as part of a plugin. For this we will edit the umbraco dashboards file.  [More about plugins](../plugins/index.md)

Open the file `/app_data/umbraco/config/umbraco.cms.dashboards.config` find the `<dashboards>`element and add the below element as a child of that.

	<add tab="Hello world" type="partialView" name="HelloWoldDashboard" />
	
This will look for the file HelloWoldDashboard.cshtml in all views folders, including the Plugins views folders, if the file is not found, umbraco will display a list of folders it tried searching.
