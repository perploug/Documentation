# List Child Pages From Current Page
This snippet below displays the child pages from a chosen node using a content picker macro parameter.

## Macro Parameters
<table>
<thead>
<tr>
<th>Show</th>
<th>Alias</th>
<th>Name</th>
<th>Type</th>
</tr>
</thead>
<tbody>
<tr>
<td>Yes</td>
<td>startNodeID</td>
<td>Start Node ID</td>
<td>Content Picker</td>
</tr>
</tbody>
</table>

##Code 
    @inherits PartialViewMacroPage
    @using Umbraco.Cms.Web
    @using Umbraco.Cms.Web.Macros
    @using Umbraco.Framework
    
    @{
        @* Get the macro parameter and check it has a value otherwise set to empty hive Id *@
        var startNodeID = String.IsNullOrEmpty(Model.MacroParameters.startNodeID) ? HiveId.Empty.ToString() : Model.MacroParameters.startNodeID;
    }
    
    @* Check that startNodeID is not an empty HiveID AND also check the string is a valid HiveID *@
    @if (startNodeID != HiveId.Empty.ToString() && HiveId.TryParse(startNodeID).Success)
    {
        @* Get the start node as a dynamic node *@
        var startNode = Umbraco.GetDynamicContentById(startNodeID);
        
        @* Check that startNode has children pages, where the property umbracoNaviHide is not True *@    
        if (startNode.Children.Where("umbracoNaviHide != @0", "True").Any())
        {
            <ul>
                @* For each child page under startNode, where the property umbracoNaviHide is not True *@
                @foreach (var page in startNode.Children.Where("umbracoNaviHide != @0", "True"))
                { 
                    <li><a href="@page.Url">@page.Name</a></li>
                }
            </ul>
        }    
    }