# List Child Pages From Current Page Ordered By Date Descending
This snippet below displays all of the child pages ordered by the created date of the node in descending order. So this list will display the newest items at the top of the list.

##Code 
    @inherits PartialViewMacroPage
    @using Umbraco.Cms.Web
    @using Umbraco.Cms.Web.Macros
    @using Umbraco.Framework
    
    
    @* Ensure that the Current Page has children, where the property umbracoNaviHide is not True *@
    @if(DynamicModel.Children.Where("umbracoNaviHide != @0", "True").Any())
    {
        <ul>            
            @* For each child page under the root node, where the property umbracoNaviHide is not True, order in descending order by date created *@
            @foreach (var childPage in DynamicModel.Children.Where("umbracoNaviHide != @0", "True").OrderByDescending("UtcCreated"))
            {
                <li>
                    <a href="@childPage.Url">@childPage.Name</a>
                </li>
            }
        </ul>
    }
