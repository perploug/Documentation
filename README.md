#Umbraco 5 Documentation project

**WORK IN PROGRESS**

The documentation project, consists of 2 main parts, "Stubs" and "Documentation". 

##Stubs
Are samplecode files for the partial and macro partial editors, heavyly commented, and available in the
umbraco core when you create new macros / partials. 

###Contributing stubs
Todo...

##[Documentation](documentation/index.md) 
is a collection of references and step-by-step guides, as well as conceptual overviews of the architecture


###Contributing documentation
All documents are written in MarkDown, using a simple structure and stored as .md files on Github.
These are then pulled to [our.umbraco.org/documentation](http://our.umbraco.org/documentation) for easy browsing. 

[Watch a video on how to contribute](http://www.screenr.com/vb78)

####Getting started with Git and GitHub

 * [Setting up Git for Windows and connecting to GitHub](http://help.github.com/win-set-up-git/)
 * [Forking a GitHub repository](http://help.github.com/fork-a-repo/)
 * [The simple gude to GIT guide](http://rogerdudler.github.com/git-guide/)

Once you're familiar with Git and GitHub, clone the repository and make your contributions.

###Discussions

* [Trello Board](https://trello.com/board/umbraco-v5-documentation-project/4f4f4d98dcf3dbda4b226e6f) - add concepts, ideas and tasks. Claim an task and start working on it!
* [JabbR Chatroom](http://jabbr.net/#/rooms/umbraco) - discuss things in real-time with the Umbraco community.

##Markdown conventions
Keep custom html to a minimum. All script and style markup are cleaned by default
Markdown syntax guid is available [here](http://daringfireball.net/projects/markdown/syntax)

###Images
Images are stored and linked relatively to .md pages, and should by convention always be in a
images folder. So to add an image to `/documentation/reference/partials/renderviewpage.md` you link it like so:

	![My Image Alt Text](images/img.jpg)

And store the image as `/documentation/reference/partials/images/img.jpg`

Images can have a max width of **650px** and max height of **600px**. Please always try to use 
the most efficiant compression, gif, png or jpg. No bmp, tiff or flash please.

###External links
Include either the complete url, or link using Markdown linking
	
	http://yahoo.com/something

	or

	[yahoo something](http://yahoo.com/something)


###internal links
If you need to link between pages, always link relatively, and include the .md extension.

	[Umbraco.Helpers](Umbraco.Helpers.md)

	or

	[Umbraco.Helpers](../../Reference/Umbraco.Helpers.md)

##Formatting
For code, indent your sample with a single tab, that will render it as `<pre><code>` tags for inline code, wrap in `chars.

Use # for headline ## for sub headers, ### for parameters (on code reference pages)

For optional parameters wrap in _ end result: `###_optionaParamter_` 


###Structure
For the documentation project, each individual topic is contained in its own folder.
Each folder must have a index.md file which links to the individual subpages, if images
are used, these must be in images folders next to the .md file referencing them releatively

* topic
	* images
		* images.jpg
	* Subtopic
		* images
		* index.md
	* index.md
	* otherpage.md

	