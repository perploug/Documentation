# RSS Feed
This snippet below allows you to generate an RSS feed on your website with some configurable options by using Macro Parameters.

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
<td>FeedItems</td>
<td>Number of Feed Items</td>
<td>Integer</td>
</tr>
<tr>
<td>Yes</td>
<td>FeedTitle</td>
<td>Feed Title</td>
<td>Textstring</td>
</tr>
<tr>
<td>Yes</td>
<td>FeedDescription</td>
<td>Feed Description</td>
<td>Textstring</td>
</tr>
<tr>
<td>Yes</td>
<td>Author</td>
<td>Author</td>
<td>Textstring</td>
</tr>
<tr>
<td>Yes</td>
<td>IsAtom</td>
<td>Is an Atom Feed?</td>
<td>True / False</td>
</tr>
</tbody>
</table>

## Credit
Credit goes to **Jorge Lusar** for help creating this snippet. Original code at [https://gist.github.com/jorgelusar](https://gist.github.com/jorgelusar)

##Code
    @inherits PartialViewMacroPage
    @using System.ServiceModel.Syndication
    @using System.Text
    @using System.Xml
    @using Umbraco.Cms.Web.Macros
    @using System.Linq
    
    @{
        var root = DynamicModel;
    
        // Get the base url to contruct the links for the feeds
        var baseUrl = new UriBuilder(Context.Request.Url.Scheme, Context.Request.Url.Host).Uri;
    
        // Get the parameters passed by the macro
        int feedItems = string.IsNullOrWhiteSpace(Model.MacroParameters.FeedItems) ? 20 : int.Parse(Model.MacroParameters.FeedItems);
        string feedTitle = string.IsNullOrWhiteSpace(Model.MacroParameters.FeedTitle) ? baseUrl.ToString() : Model.MacroParameters.FeedTitle;
        string feedDescription = string.IsNullOrWhiteSpace(Model.MacroParameters.FeedDescription) ? baseUrl.ToString() : Model.MacroParameters.FeedDescription;
        string author = string.IsNullOrWhiteSpace(Model.MacroParameters.Author) ? "admin" : Model.MacroParameters.Author;
        bool isAtom;
        bool.TryParse(Model.MacroParameters.IsAtom, out isAtom);
    
        // Sort the items by CreateDate and limit the number of feeds based on the feedItems parameter
        // Notice that you could sort by "UpdateDate" too
        IList<dynamic> news = root.Children.OrderBy("CreateDate");
    
        // Let's create a list of Syndication Items
        var items = new List<SyndicationItem>();
    
        foreach (var item in news.Take(feedItems))
        {
          // Get the title of the item. 
          string title = item.Name;
    
          // Get the content from the body text and decode it
          string content = item.BodyText;
    
          // Create the link based on the baseUrl and the item url
          Uri itemAlternateLink = new Uri(baseUrl, item.Url);
    
          // Get the updateDate
          DateTime updatedTime = DateTime.Parse(item.UpdateDate.ToString());
    
          // Add the SyndicationItem to the list
          SyndicationItem syndicationItem = new SyndicationItem
          {
            Id = itemAlternateLink.AbsoluteUri,
            Title = SyndicationContent.CreatePlaintextContent(title),
            Content = SyndicationContent.CreateHtmlContent(content)
          };
    
          // Set the PublishDate or LastUpdatedTime based on the information whether
          // the feed will be a rss or a atom
          if (isAtom)
          {
            syndicationItem.LastUpdatedTime = updatedTime;
          }
          else
          {
            syndicationItem.PublishDate = updatedTime;
          }
    
          // Add the itemAlternateLink
          syndicationItem.Links.Add(SyndicationLink.CreateAlternateLink(itemAlternateLink));
    
          // Add the item
          items.Add(syndicationItem);
        }
    
        // Get the feed link
        var feedLink = new Uri(baseUrl, Context.Request.RawUrl);
    
        // Get the UpdateDate for the first item on the list
        var feedDate = DateTime.Parse(news.First().UpdateDate.ToString());
    
        // Create the feed
        var feed = new SyndicationFeed(feedTitle, feedDescription, feedLink, feedLink.AbsoluteUri, feedDate, items);
    
        // Improved interoperability with feed readers by implementing atom:link with rel="self"
        feed.Links.Add(SyndicationLink.CreateSelfLink(feedLink));
    
        // Let's save the content to a MemoryStream. You could use StringWriter too but you will have to deal with UTF-16 encoding
        using (var ms = new MemoryStream())
        {
          // Ensure the writer is UTF8 and nicely indented
          using (var writer = new XmlTextWriter(ms, Encoding.UTF8) { Formatting = Formatting.Indented })
          {
            // Write <?xml version="1.0" encoding="utf-8"?>
            writer.WriteStartDocument();
    
            // Save as atom or as rss
            if (isAtom)
            {
              // This feed won't validate unless you add an author and name
              feed.Authors.Add(new SyndicationPerson { Name = author });
              feed.SaveAsAtom10(writer);
            }
            else
            {
              feed.SaveAsRss20(writer);
            }
          }
    
          var sb = new StringBuilder(Encoding.UTF8.GetString(ms.ToArray()));
    
          if (!isAtom)
          {
            // Although this feed is valid, interoperability with feed readers could be improved by avoiding Namespace Prefix: a10
            sb.Replace("xmlns:a10", "xmlns:atom").Replace("a10:", "atom:");
          }
    
          // Ensure the content type is correct
          Context.Response.ContentType = isAtom ? "application/atom+xml" : "application/rss+xml";
    
          // Write the content
          Context.Response.Write(sb.ToString());
        }
    }