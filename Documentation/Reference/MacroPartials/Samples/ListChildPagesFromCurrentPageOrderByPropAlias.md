# List Child Pages From Current Page Ordered By Property Alias
This snippet below displays the child pages from a chosen node using a content picker macro parameter.
This snippet below displays all of the child pages ordered by a property on all the child pages in ascending order. This is done by passing in the property alias to sort in as a macro parameter.


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
<td>propertyAlias</td>
<td>Property Alias</td>
<td>Textstring</td>
</tr>
</tbody>
</table>

##Code 
    @inherits PartialViewMacroPage
    @using Umbraco.Cms.Web
    @using Umbraco.Cms.Web.Macros
    @using Umbraco.Framework
    
    @{
        @* Get the content type alias we want to filter on from the macro parameter *@
        var propertyAlias = Model.MacroParameters.propertyAlias;
    }
    
    
    @* Ensure that the Current Page has children, where the property umbracoNaviHide is not True *@
    @if(DynamicModel.Children.Where("umbracoNaviHide != @0", "True").Any())
    {
        <ul>            
            @* For each child page under the root node, where the property umbracoNaviHide is not True, order in descending order by date created *@
            @foreach (var childPage in DynamicModel.Children.Where("umbracoNaviHide != @0", "True").OrderBy(propertyAlias))
            {
                <li>
                    <a href="@childPage.Url">@childPage.Name</a>
                </li>
            }
        </ul>
    }
