#Dashboards

##[Childaction-based dashboard](ChildAction.md)
A Child action based dashboard has it's own dedicated Controller and view, and works like any other standard Controller. If you are building a dashboard containing forms posting back to the dashboard, you will require a Child action based dashboard. 

##[PartialView-based dashboards](ParetialView.md)
A view based dashboard, uses a single cshtml file to present content on the dashboard. It is simple to setup, but without the power of a controller behind it. View-based dashboards are primarily used for fetching and displaying data only.

##Configuration
To enable a dashboard, you are required to register it in the umbraco configuration. 

You have 2 options for this, either register the dashboard directly in the core dashboard configuration file which is located in ` /app_data/umbraco/config/umbraco.cms.dashboards.config` or by including the dashboard in a plugin. [More about plugins](../plugins/index.md)

###Dashboard config structure
` /app_data/umbraco/config/umbraco.cms.dashboards.config`  sticks to the below structure. Its root elements is `<dashboard-groups>` containing a collection of group elements. Each `<group>` specifies on what applications the `<group>` should be displayed. Each group contains a list of dashboards which are what specifies the actual visual elements on the dashboard. 

**So to summerize**: A dashboard group is configured, to specify what applications have access to the `<group>`. Furthermore, the `<group>` specifies what visual components is display (`<dashboards>`)

    <?xml version="1.0"?>
	<dashboard-groups>
	  <!-- This group shows the "Get started" dashboards -->
	  <group>
	    <applications>
	      <add app="members" />
	    </applications>
	    <dashboards>
	      <add tab="Get started" type="partialView" name="StartHere" />
		<add tab="Get started" type="childAction" name="NameOfController.Action" /> 	      
	    </dashboards>
	  </group> 
	</dashboard-groups> 


###Dashboard config options
Dashboards are adding under the `<dashboards>` element as a `<add>` element

	<add tab="Get started" type="partialView" name="StartHere" />

####tab
Sets the tab, the component should be displayed on. If the tab is already used by another dashboard, this dashboard is appended to it, otherwise, the tab is created.

####type
A dashboard type can either be based on a [partialView](partialView.md), or a [childAction](childaction.md)

####name
The meaning of this option, depends on the type of the dashboard. 

**For PartialView based dashboards**, it refers to the name of the view file used. (without path or extension)

	<add tab="Get started" type="partialView" name="StartHere" />
	
Refers to using the file starthere.cshtml which umbraco will search for in appropriate locations. The cshtml/vbhtml file, should therefore be in one of the following locations (or in a sub-direcory): 

* ~/Areas/Umbraco/Views/DashboardEditor/
* ~/Areas/Umbraco/Views/Shared/ 
* ~/Views/DashboardEditor/
* ~/Views/Shared/
* ~/Views/Umbraco/
* ~/Umbraco/Views/Dashboards/
* ~/App_Plugins/Packages/YourPluginName/Views/

Umbraco searches other locations as well, which are displayed if it cannot find a partial view file.

**For ChildAction based dashboarsd**, name refers to the Controller and Action which are used to render the dashboard. The action must return a `PartialViewResult` , the controller must be of type a `DashboardEditorController`

	<add tab="Get started" type="childAction" name="LicenseDashboard.DisplayDashboard" />

Refers to the class `LicenseDashboardController` and in that class, it looks for a method, named `DisplayDashboard`, which returns a `PartialViewResult` There are some additional steps
required to setup a ChildAction based dashboard, which you can read about [here](ChildAction.md) 
