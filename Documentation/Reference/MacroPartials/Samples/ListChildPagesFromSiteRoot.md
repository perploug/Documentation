# List Child Pages From Site Root
This snippet displays the first level of child pages from the site root node in an unordered list if it has any.

##Code 
    @inherits PartialViewMacroPage
    @using Umbraco.Cms.Web
    @using Umbraco.Cms.Web.Macros
    @using Umbraco.Framework
    
    @{    
        @* Walk up the tree from the current page to get the root node *@
        var rootNode = DynamicModel.AncestorsOrSelf.Last();
    }
    
    
    @* Ensure that the rootNode has children, where the property umbracoNaviHide is not True *@
    @if (rootNode.Children.Where("umbracoNaviHide != @0", "True").Any())
    {
        <ul>            
            @* For each child page under the root node, where the property umbracoNaviHide is not True *@
            @foreach(var childPage in rootNode.Children.Where("umbracoNaviHide != @0", "True"))
            {
                <li>
                    <a href="@childPage.Url">@childPage.Name</a>
                </li>
            }
        </ul>
    }