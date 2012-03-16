# List Descendant Pages from Current Page
This snippet displays all of the child pages and their children (descendants) in a nested unordered list.

The snippet makes use of a helper, in order for us to help do a recursive loop, to list out the child pages of a child and so on and so forth.

##Code 
    @inherits PartialViewMacroPage
    @using Umbraco.Cms.Web
    @using Umbraco.Cms.Web.Macros
    @using Umbraco.Framework
    
    
    @* Ensure that the Current Page has children, where the property umbracoNaviHide is not True *@
    @if (DynamicModel.Children.Where("umbracoNaviHide != @0", "True").Any())
    {
        @* Get the first page of the children, where the property umbracoNaviHide is not True *@
        var naviLevel = DynamicModel.Children.Where("umbracoNaviHide != @0", "True").First().Level;
        
        @* Add in level for a CSS hook *@
        <ul class="level-@naviLevel">            
            @* For each child page under the root node, where the property umbracoNaviHide is not True *@
            @foreach (var childPage in DynamicModel.Children.Where("umbracoNaviHide != @0", "True"))
            {
                <li>
                    <a href="@childPage.Url">@childPage.Name</a>
    
                    @* if the current page has any children, where the property umbracoNaviHide is not True *@
                    @if (childPage.Children.Where("umbracoNaviHide != @0", "True").Any())
                    {                    
                        @* Call our helper to display the children *@
                        @childPages(childPage.Children)
                    }
                </li>
            }
        </ul>
    }
    
    
    @helper childPages(dynamic pages)
        {
        @* Ensure that we have a collection of pages *@
        if (pages.Any())
        {
            @* Get the level of the first page *@
            var naviLevel = pages.First().Level;
            
            @* Add in level for a CSS hook *@
            <ul class="level-@(naviLevel)">
                @foreach (var page in pages.Where("umbracoNaviHide != @0", "True"))
                {
                    <li>
                        <a href="@page.Url">@page.Name</a>
                        
                        @* if the current page has any children, where the property umbracoNaviHide is not True *@
                        @if (page.Children.Where("umbracoNaviHide != @0", "True").Any())
                        {                        
                            @* Call our helper to display the children *@
                            @childPages(page.Children)
                        }
                    </li>
                }
            </ul>
        }
    }