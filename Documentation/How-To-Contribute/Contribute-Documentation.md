#Contribute Documentation for Umbraco 5

Our documentation is a team-led, community-powered project. We have a dedicated number of people who regularly work on the documentation each week in the first half of 2012, and in addition the project is open to anyone who wishes to help out. 

Got a snippet you think would be useful, or had to find out how to do something the hard way and want to help others avoid the same hours of searching? Our collaboration project is hosted on [Github](https://github.com/umbraco/documentation) and open for pull requests from anyone. We will love you for it. 

**WORK IN PROGRESS**

#Reading & using the docs
This is the documentation project for Umbraco 5. The scope of this project is to provide overviews of concepts, tutorials, example code, and links to API reference.
This is a work in progress at the moment, and we welcome valuable contributions from anyone willing to help. For details on how to contribute, see further down this page.

To allow us to maintain documentation specific to each version of Umbraco 5, we have branches for each release starting from 5.0.1.

Once you have selected a branch, you're good to go. Our project consists of 2 main parts, "Stubs" and "Documentation". 

#What's in these guides

##Stubs
This contains sample code files or "snippets" that you may find useful when building out your site. In Umbraco terminology, they are used for the "partial" and "macro partial" editors. They are heavily commented, and will be available in the Umbraco release too when you create new macros or partials. 

##Documentation
[Documentation (available here)](../index.md) is a collection of references and step-by-step guides, as well as conceptual overviews of the architecture.

#Contributing to the guides
To contribute to either the documentation or stubs, you can fork & clone our repository, make your edits, and simply push back to gitHub and send us a pull request. All items that get pulled into the main repository will automatically get pushed to [our.umbraco.org/documentation](http://our.umbraco.org/documentation).

####Getting started with Git and GitHub
 * [Setting up Git for Windows and connecting to GitHub](http://help.github.com/win-set-up-git/)
 * [Forking a GitHub repository](http://help.github.com/fork-a-repo/)
 * [The simple guide to GIT](http://rogerdudler.github.com/git-guide/)

For more details on the process, [watch our video on how to contribute](http://www.screenr.com/vb78).

##Repository organisation
So that we can target documentation for each release of Umbraco 5, there will be a branch for each released version.
When making corrections or edits to documentation against one revision, if those updates are relevant to another release, merge those specific changes to the other relevant branches.
No active work on the documentation should be done on the 'master' branch.

###Contributing documentation
All documents are written in Markdown, using a simple structure and stored as .md files.
These are then pulled to [our.umbraco.org/documentation](http://our.umbraco.org/documentation) for easy browsing. 

First fork and clone the repository so that you have your own working copy. Then choose the minimum version of Umbraco 5 to which you know your edits apply. Then make your changes on the git branch for this version. Once you are happy with your edits, use github to issue a "pull request" which means your edits will be reviewed, and once accepted, merged into the main repository. Everything in the main repository will make it onto the [our.umbraco.org/documentation](http://our.umbraco.org/documentation) site, which is why we have chosen a pull request workflow to keep everything simple and straightforward.

###Contributing stubs
The code stub examples are written in Razor and stored as .cshtml files on in the git repository.

##Planning & discussions

If you would like to get more involved in the discussions around the Umbraco Documentation project, please visit the following places:

* [Trello Board](https://trello.com/board/umbraco-v5-documentation-project/4f4f4d98dcf3dbda4b226e6f) - our planning board full of our backlog and work-in-progress items. Browse this board to get an idea of what needs more work, or what someone else has already started. Join the board to add concepts, ideas and tasks. Claim a task and start working on it!
* [JabbR Chatroom](http://jabbr.net/#/rooms/umbraco) - discuss things in real-time with the Umbraco community.


#Markdown conventions
Keep custom HTML to a minimum. All script and style mark-up are cleaned by default.
For reference, the [Markdown syntax guide](http://daringfireball.net/projects/markdown/syntax) is available.

##Images
Images are stored and linked relatively to .md pages, and should by convention always be in an
`images` folder. So to add an image to `/documentation/reference/partials/renderviewpage.md` you link it like so:

	![My Image Alt Text](images/img.jpg)

And store the image as `/documentation/reference/partials/images/img.jpg`

Images can have a maximum width of **650px** and maximum height of **600px**. Please always try to use 
the most efficient compression, `gif`, `png` or `jpg`. No `bmp`, `tiff` or `swf` (Flash), please.

##External links
Include either the complete URL, or link using Markdown:
	
	http://yahoo.com/something

	or

	[yahoo something](http://yahoo.com/something)


##Internal links
If you need to link between pages, always link relatively, and include the .md extension.

	[Umbraco.Helpers](Umbraco.Helpers.md)

	or

	[Umbraco.Helpers](../../Reference/Umbraco.Helpers.md)

##Formatting code
Indent your sample with a single tab, which will cause it to be rendered as `<pre><code>` tags.
For inline code, wrap in ` (backtick) chars.

Use # for the headline, ## for sub headers and ### for parameters (on code reference pages)

For optional parameters wrap in _ (underscore) - end result: `###_optionalParameter_` 

##Structure
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

	