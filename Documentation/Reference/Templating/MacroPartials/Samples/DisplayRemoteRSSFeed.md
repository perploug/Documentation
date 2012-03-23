# Display Remote RSS Feed
This snippet below allows you to display a remote RSS feed on your website with some configurable options by using Macro Parameters.


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
<td>remoteURL</td>
<td>Remote URL</td>
<td>Textstring</td>
</tr>
</tbody>
</table>

##Code 
    @inherits PartialViewMacroPage
    @using Umbraco.Cms.Web.Macros
    
    @* Added these in *@
    @using System.ServiceModel.Syndication
    @using System.Xml;
    
    @{
      // Get the remote url 
      var remoteUrl = String.IsNullOrEmpty(Model.MacroParameters.remoteURL) ? "http://feeds.feedburner.com/umbracoblog" : Model.MacroParameters.remoteURL;
    
      // Get the web request 
      var request = WebRequest.Create(remoteUrl);
    
      // Set the timeout to 3 seconds (3000ms)
      request.Timeout = 3000;
    
      // Let's put it in a try/catch because the web request could throw and error (timeout, 500, etc)
      try
      {
          using (var response = request.GetResponse())
          {
              using (var stream = response.GetResponseStream())
              {
                  if (stream != null)
                  {
                      var reader = XmlReader.Create(stream);
                      var feed = SyndicationFeed.Load(reader);
                      
                      foreach (SyndicationItem item in feed.Items)
                      {
                          var link          = item.Links.FirstOrDefault().GetAbsoluteUri();
                          var description   = item.ElementExtensions.ReadElementExtensions<string>("encoded", "http://purl.org/rss/1.0/modules/content/").FirstOrDefault();
                          var pubDate       = item.PublishDate;
                          
                          <div class="rssItem">
                            <h1><a href="@link">@item.Title.Text</a></h1>
                            <em>@pubDate.ToString("dd/MM/yy @ HH:mm")</em>
                            @Html.Raw(description)
                          </div>
                        }
                    }
                }
            }
        }
        catch (Exception ex)
        {
            // Log the error or do something with it...
        }
    }