# Twitter
The snippet below allows you to display tweets from a Twitter account with some configurable options by using Macro Parameters.

This snippet works by remotely fetching the JSON from Twitter's API server and then allowing us to iterate over the JSON with Razor. Allowing us to display any information from the JSON feed.

The snippet outputs the following, but you could easily customize it to your own needs:

+ Twitter Name
+ Avatar
+ Tweet - Parsing of the following:
	+ URLs
	+ User Mentions
	+ Hash Tags
+ Display any attached images
+ Display a static Google Map image if Tweet has Geo location data


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
<td>twitterUsername</td>
<td>Twitter Username</td>
<td>Textstring</td>
</tr>
<tr>
<td>Yes</td>
<td>includeRTs</td>
<td>Include Retweets?</td>
<td>True / False</td>
</tr>
<tr>
<td>Yes</td>
<td>excludeReplies</td>
<td>Exclude Replies?</td>
<td>True / False</td>
</tr>
<tr>
<td>Yes</td>
<td>noTweets</td>
<td>Number of Tweets</td>
<td>Integer</td>
</tr>
</tbody>
</table>

##Code 
    @inherits PartialViewMacroPage
    @using Umbraco.Cms.Web
    @using Umbraco.Cms.Web.Macros
    @using Umbraco.Framework
    
    @{   
        
        @* Macro Param: Twitter Username that defaults to 'umbraco' if empty *@
        var twitterUsername = String.IsNullOrEmpty(Model.MacroParameters.twitterUsername) ? "umbracoproject" : Model.MacroParameters.twitterUsername;
    
        @* Macro Param: Include Retweets? Defaults to false *@
        var includeRTs = String.IsNullOrEmpty(Model.MacroParameters.includeRTs) ? false : Model.MacroParameters.includeRTs;
    
        @* Macro Param: Exclude Replies? Defaults to false *@
        var excludeReplies = String.IsNullOrEmpty(Model.MacroParameters.excludeReplies) ? false : Model.MacroParameters.excludeReplies;
    
        @* Macro Param: Number of tweets to display. Defaults to 1 *@
        var noTweets = String.IsNullOrEmpty(Model.MacroParameters.noTweets) | Convert.ToInt32(Model.MacroParameters.noTweets) == 0 ? 1 : Model.MacroParameters.noTweets;
    
        @* Twitter JSON URL *@
        var twitterURL = string.Format(
                "https://api.twitter.com/1/statuses/user_timeline.json?screen_name={0}&include_rts={1}&exclude_replies={2}&include_entities=1&count={3}",
                twitterUsername,
                includeRTs,
                excludeReplies,
                noTweets);                
                
    }
    
    @* Fetch the JSON from Twitter's API *@
    @using (var client = new System.Net.WebClient())
    {
        @* Fetch the JSON from Twitter *@
        var response = client.DownloadString(new Uri(twitterURL));
    
        @* Decode the JSON so we can iterate over it *@
        var tweets = Json.Decode(response);
        
        <ul>    
            @foreach (var tweet in tweets)
            {
                <li>
                    @* Tweet with formatted links *@
                    <h3>@formatLinks(tweet.text, tweet.entities)</h3>
    
                    <p>
                        @* Format Tweet Date and output as 24/03/12 @ 14:05 *@
                        <em>@formatDate(tweet.created_at).ToString("dd/MM/yy @ HH:mm")</em>
                    </p>
    
                    <p>
                        @* Profile Image *@
                        <img src="@tweet.user.profile_image_url" alt="@tweet.user.Name" />
                    
                        @* Your real name (not profile name) *@
                        <strong>@tweet.user.name</strong>
                    </p>
    
                    @* Google Map (if tweet has geo info) *@
                    @if (tweet.geo != null)
                    {
                        @Html.Raw(displayMap(tweet.geo));
                    }                
    
                    @* Dislay Image (if tweet has image attached) *@
                    @if (tweet.entities.media != null)
                    {
                        <a href="@tweet.entities.media[0].media_url" target="_blank">
                            <img src="@tweet.entities.media[0].media_url:thumb" />
                        </a>
                    }
                </li>
            }
        </ul>
    }
    
    @functions
    {
        DateTime formatDate(string twitterDate)
        {
            //Example tweet date
            //Fri Mar 02 16:09:35 +0000 2012
            //ddd MMM dd HH:mm:ss zz00 yyyy
            DateTime tweetDate = DateTime.ParseExact(twitterDate, "ddd MMM dd HH:mm:ss zz00 yyyy", null);
    
            return tweetDate;
        }
    
        String displayMap(dynamic geo)
        {
            //Get the lat & long values
            var tweetLat    = geo.coordinates[0];
            var tweetLong   = geo.coordinates[1];
    
            //Format the string to return the image
            var googleMap   = string.Format("http://maps.googleapis.com/maps/api/staticmap?center={0},{1}&zoom=14&size=250x250&maptype=roadmap&sensor=false&markers={0}, {1}", tweetLat, tweetLong);
            var mapImage    = string.Format("<img src='{0}' alt='map' />", googleMap);
    
            return mapImage;
        }
    
        IHtmlString formatLinks(string tweet, dynamic entities)
        {
            //A List of tweet entities so we can sort all of them
            IList<tweetEntity> tweetEntities = new List<tweetEntity>();
    
            //Get URLs
            var links = entities.urls;
    
            //Check we have links to loop over
            if (links != null)
            {
                //For each link in the collection of links
                foreach (var link in links)
                {
                    var startPosition   = link.indices[0];
                    var endPosition     = link.indices[1];
                    var length          = endPosition - startPosition;
                    
                    var url             = link.url;         //The short t.co link
                    var displayURL      = link.display_url; //A friendly version of the full link (may be truncated)
    
                    var newText = string.Format("<a href='{0}' target='_blank'>{1}</a>", url, displayURL);
                    var oldText = tweet.Substring(startPosition, length);
                    
    
                    //Create a new entity
                    tweetEntity entity      = new tweetEntity();
                    entity.startPosition    = startPosition;
                    entity.endPosition      = endPosition;
                    entity.newText          = newText;
                    entity.oldText          = oldText;
    
                    //Add it to the collection
                    tweetEntities.Add(entity);
                }
            }
    
    
            //Get user mentions (e.g.: @umbraco)
            var mentions = entities.user_mentions;
    
            //Check we have mentions to loop over
            if (mentions != null)
            {
                //For each mention in the collection of mentions
                foreach (var mention in mentions)
                {
                    var startPosition   = mention.indices[0];
                    var endPosition     = mention.indices[1];
                    var length          = endPosition - startPosition;
                    
                    var username        = mention.screen_name;
    
                    var newText = string.Format("<a href='http://twitter.com/{0}' target='_blank'>@{0}</a>", username);
                    var oldText = tweet.Substring(startPosition, length);
    
                    //Create a new entity
                    tweetEntity entity      = new tweetEntity();
                    entity.startPosition    = startPosition;
                    entity.endPosition      = endPosition;
                    entity.newText          = newText;
                    entity.oldText          = oldText;
    
                    //Add to collection
                    tweetEntities.Add(entity);
                }
            }
    
            //Get hashtags
            var hashtags = entities.hashtags;
    
            //Check we have hash to loop over
            if (hashtags != null)
            {
                foreach (var hash in hashtags)
                {
                    var startPosition   = hash.indices[0];
                    var endPosition     = hash.indices[1];
                    var length          = endPosition - startPosition;
                    var hashtag         = hash.text;
    
                    var newText         = string.Format("<a href='http://twitter.com/search/%23{0}' target='_blank'>#{0}</a>", hashtag);
                    var oldText         = tweet.Substring(startPosition, length);
    
                    //Create a new entity
                    tweetEntity entity      = new tweetEntity();
                    entity.startPosition    = startPosition;
                    entity.endPosition      = endPosition;
                    entity.newText          = newText;
                    entity.oldText          = oldText;
    
                    //Add to collection
                    tweetEntities.Add(entity);
                }
            }
    
            //For each item in the tweet entities in reverse order
            //If we update the string in reverse order the remaining start/end indexes will still be correct
            foreach (var item in tweetEntities.OrderByDescending(x => x.startPosition))
            {
                //Let's update the tweet text
                tweet = tweet.Replace(item.oldText, item.newText);
            }
    
            
            //Return the new tweet with all the links added in
            return Html.Raw(tweet);
        }
        
        public class tweetEntity
        {
            public int startPosition    { get; set; }
            public int endPosition      { get; set; }
            public string oldText       { get; set; }
            public string newText       { get; set; }
        }
    }