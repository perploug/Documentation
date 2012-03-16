# List All Pages (Sitemap)
This snippet below displays all of the pages from the site root and their children (descendants) in a nested unordered list.

This snippet makes use of a helper, in order for us to help do a recursive loop, to list out the child pages of a child and so on and so forth. The obvious use for this is a sitemap.

##Code 
    @inherits PartialViewMacroPage
    @using Umbraco.Cms.Web
    @using Umbraco.Cms.Web.Macros
    @using Umbraco.Framework
    
    @{    
        @* Walk up the tree from the current page to get the root node *@
        var rootNode = DynamicModel.AncestorsOrSelf.Last();
    }
    
    @* Ensure that the Root Node has children, where the property umbracoNaviHide is not True *@
    @if (rootNode.Children.Where("umbracoNaviHide != @0", "True").Any())
    {
        @* Get the level of the first childnode, where the property umbracoNaviHide is not True *@
        var naviLevel = rootNode.Children.Where("umbracoNaviHide != @0", "True").First().Level;
        
        @* Add in level for a CSS hook *@
        <ul class="level-@naviLevel">            
            @* For each child page under the root node, where the property umbracoNaviHide is not True *@
            @foreach (var childPage in rootNode.Children.Where("umbracoNaviHide != @0", "True"))
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
            @* Get the first page in pages and get the level *@
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