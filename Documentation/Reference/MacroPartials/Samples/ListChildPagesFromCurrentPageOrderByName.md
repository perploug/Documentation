# List Child Pages From Current Page Ordered By Name
This snippet below displays all of the child pages ordered by the node name in ascending order. So this list will display the list in alphabetical order A to Z.


##Code 
    @inherits PartialViewMacroPage
    @using Umbraco.Cms.Web
    @using Umbraco.Cms.Web.Macros
    @using Umbraco.Framework
    
    
    @* Ensure that the Current Page has children, where the property umbracoNaviHide is not True *@
    @if(DynamicModel.Children.Where("umbracoNaviHide != @0", "True").Any())
    {
        <ul>            
            @* For each child page under the root node, where the property umbracoNaviHide is not True, order in ascending order by name of node *@
            @foreach (var childPage in DynamicModel.Children.Where("umbracoNaviHide != @0", "True").OrderBy("Name"))
            {
                <li>
                    <a href="@childPage.Url">@childPage.Name</a>
                </li>
            }
        </ul>
    }
