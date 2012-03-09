# List Ancestors From Current Page (Breadcrumb)
This snippet will list all the ancestor pages of the current page, which is more commonly known as a breadcrumb.

##Code 
    @inherits PartialViewMacroPage
    @using Umbraco.Cms.Web
    @using Umbraco.Cms.Web.Macros
    @using Umbraco.Framework
    
    
    @* Check the current page has ancestors *@
    @if (DynamicModel.Ancestors.Any())
    { 
        <ul>
            @* For each page in the ancestors collection which have been ordered by Level (so we start with the highest top node first) *@
            @foreach (var page in DynamicModel.Ancestors.OrderBy("Level"))
            {
                <li><a href="@page.Url">@page.Name</a> &raquo;</li>
            }
    
            @* Display the current page as the last item in the list *@
            <li>@DynamicModel.Name</li>
        </ul>
    }