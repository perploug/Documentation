# List Child Pages From Current Page
The snippet below displays the first level of child pages from the current page (if it has any) in an unordered list.

##Code 
    @inherits PartialViewMacroPage
    @using Umbraco.Cms.Web
    @using Umbraco.Cms.Web.Macros
    @using Umbraco.Framework
    
    
    @* Ensure that the Current Page has children, where the property umbracoNaviHide is not True *@
    @if(DynamicModel.Children.Where("umbracoNaviHide != @0", "True").Any())
    {
        <ul>            
            @* For each child page under the root node, where the property umbracoNaviHide is not True *@
            @foreach (var childPage in DynamicModel.Children.Where("umbracoNaviHide != @0", "True"))
            {
                <li>
                    <a href="@childPage.Url">@childPage.Name</a>
                </li>
            }
        </ul>
    }