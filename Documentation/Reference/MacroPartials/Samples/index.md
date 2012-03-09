# Macro Razor Partial View Samples
This document outlines common code snippets that you will use in an Umbraco website and is a great start for you to learning Umbraco.

Each code snippet listed here is heavily commented and should be easy to read for you to understand what is happening in each example, in addition each snippet will make use of the Umbraco convention umbracoNaviHide

##Listing Pages
These group of snippets will list pages in different ways.

### Listing Child Pages from Current Page

### Listing Child Pages from Current Page by Content Type

### Listing Child Pages from a Chosen Node

### Listing Child Pages from the Site Root

### Listing Descendants from Current Page


## Ordering
These snippets help you to display items from 

### Listing Child Pages from Current Page In Descending Order by Date Created

### Listing Child Pages from Current Page In Ascending Order by Node Name

### Listing Child Pages from Current Page In Descending Order by Property Alias

##Media
These snippets are specific to working with media items

### List Child Media Items from a Media Folder
This snippet below displays the child media items from a chosen media folder using a media picker macro parameter.

This snippet would be useful for outputting a list of images from a folder to use with a jQuery carousel component.

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
<td>mediaFolderID</td>
<td>Media Folder ID</td>
<td>Media Picker</td>
</tr>
</tbody>
</table>

    @inherits PartialViewMacroPage
    @using Umbraco.Cms.Web
    @using Umbraco.Cms.Web.Macros
    @using Umbraco.Framework
     
    @{
     
        @* Get the macro parameter and check it has a value otherwise set to empty hive Id *@
        var mediaFolderID = String.IsNullOrEmpty(Model.MacroParameters.mediaFolderID) ? HiveId.Empty.ToString() : Model.MacroParameters.mediaFolderID;
    }
     
    @* Check that mediaFolderID is not an empty HiveID AND also check the string is a valid HiveID *@
    @if (mediaFolderID != HiveId.Empty.ToString() && HiveId.TryParse(mediaFolderID).Success)
    {
         @* Get the media folder as a dynamic node *@
        var mediaFolder = Umbraco.GetDynamicContentById(mediaFolderID);
        
        @* Check that mediaFolder has children media items *@    
        if (mediaFolder.Children.Any())
        {
            <ul>
                @* for each item in children of the selected media folder *@
                @foreach (var mediaItem in mediaFolder.Children)
                {
                    @*
                    ** Get the image URL **
                    The HiveID of the selected media item from the media picker
                    Then the string property alias of the upload data type on the media type
                    *@
                    var imageURL = Umbraco.GetMediaUrl(mediaItem.Id, "uploadedFile");
     
                    @* Get the name of the media item *@
                    var mediaName = mediaItem.Name;
                
                    <li><img src="@imageURL" alt="@mediaName" /></li>
                }
            </ul>
        }
    }
    
