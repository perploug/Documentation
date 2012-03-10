#Listing Child Pages from Current Page by Content Type
The snippet below displays the child pages filtered by the Content Type Alias from the current page.

*The Content Type Alias is a new name for what was previously referred to as Document Type Alias*

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
<td>contentTypeAlias</td>
<td>Content Type Alias</td>
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
        var contentTypeAlias = Model.MacroParameters.contentTypeAlias;
    }
    
    @* Ensure that the rootNode has children, where the property umbracoNaviHide is not True and content type of node matches our macro param *@
    @if (DynamicModel.Children.Where("umbracoNaviHide != @0 && ContentTypeAlias == @1", "True", contentTypeAlias).Any())
    {
        <ul>            
            @* For each child page under the root node, where the property umbracoNaviHide is not True and content type of node matches our macro param *@
            @foreach (var childPage in DynamicModel.Children.Where("umbracoNaviHide != @0 && ContentTypeAlias == @1", "True", contentTypeAlias))
            {
                <li>
                    <a href="@childPage.Url">@childPage.Name</a>
                </li>
            }
        </ul>
    }
