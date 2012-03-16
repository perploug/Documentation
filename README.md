#Umbraco 5 Documentation project

**WORK IN PROGRESS**

#Reading & using the docs
The documentation project, consists of 2 main parts, "Stubs" and "Documentation". 

##Stubs
All sample code files for the partial and macro partial editors, heavily commented, and will be available in the
Umbraco core when you create new macros / partials. 

##[Documentation](Documentation/index.md)
Documentation is a collection of references and step-by-step guides, as well as conceptual overviews of the architecture.

#Contributing
To contribute to either the documentation or stubs, you can clone our repository and work on the files.

####Getting started with Git and GitHub
 * [Setting up Git for Windows and connecting to GitHub](http://help.github.com/win-set-up-git/)
 * [Forking a GitHub repository](http://help.github.com/fork-a-repo/)
 * [The simple gude to GIT guide](http://rogerdudler.github.com/git-guide/)

Once you're familiar with Git and GitHub, clone the repository and make your contributions.

For more details on the process, [watch our video on how to contribute](http://www.screenr.com/vb78).

##Repository organisation
So that we can target documentation for each release of Umbraco 5, there will be a branch for each released version.
When making corrections or edits to documentation against one revision, if those updates are relevant to another release, merge those specific changes to the other relevant branches.
No active work on the documentation should be done on the 'master' branch.

###Contributing stubs
The code stub examples are written in Razor and stored as .cshtml files on GitHub.

###Contributing documentation
All documents are written in Markdown, using a simple structure and stored as .md files on GitHub.
These are then pulled to [our.umbraco.org/documentation](http://our.umbraco.org/documentation) for easy browsing. 

###Discussions
If you would like to get more involved in the discussions around the Umbraco Documentation project, please visit the following places:

* [Trello Board](https://trello.com/board/umbraco-v5-documentation-project/4f4f4d98dcf3dbda4b226e6f) - add concepts, ideas and tasks. Claim a task and start working on it!
* [JabbR Chatroom](http://jabbr.net/#/rooms/umbraco) - discuss things in real-time with the Umbraco community.

***

##Markdown conventions
Keep custom HTML to a minimum. All script and style mark-up are cleaned by default.
For reference, the [Markdown syntax guide](http://daringfireball.net/projects/markdown/syntax) is available.

###Images
Images are stored and linked relatively to .md pages, and should by convention always be in an
`images` folder. So to add an image to `/documentation/reference/partials/renderviewpage.md` you link it like so:

	![My Image Alt Text](images/img.jpg)

And store the image as `/documentation/reference/partials/images/img.jpg`

Images can have a maximum width of **650px** and maximum height of **600px**. Please always try to use 
the most efficient compression, `gif`, `png` or `jpg`. No `bmp`, `tiff` or `swf` (Flash), please.

###External links
Include either the complete URL, or link using Markdown:
	
	http://yahoo.com/something

	or

	[yahoo something](http://yahoo.com/something)


###Internal links
If you need to link between pages, always link relatively, and include the .md extension.

	[Umbraco.Helpers](Umbraco.Helpers.md)

	or

	[Umbraco.Helpers](../../Reference/Umbraco.Helpers.md)

##Formatting
For code, indent your sample with a single tab, that will render it as `<pre><code>` tags for inline code, wrap in ` (backtick) chars.

Use # for the headline, ## for sub headers and ### for parameters (on code reference pages)

For optional parameters wrap in _ (underscore) - end result: `###_optionalParameter_` 


###Structure
For the documentation project, each individual topic is contained in its own folder.
Each folder must have an `index.md` file which links to the individual sub-pages, if images
are used, these must be in `images` folders next to the .md file referencing them relatively.

* topic
	* images
		* images.jpg
	* Subtopic
		* images
		* index.md
	* index.md
	* otherpage.md

	