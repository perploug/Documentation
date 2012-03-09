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
These are then pulled to our.umbraco.org/documentation for easy browsing. 


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

	[yahoo something](Http://yahoo.com/something)


###internal links
If you need to link between pages, always link relatively, and include the .md extension.

	[Umbraco.Helpers](Umbraco.Helpers.md)

	or

	[Umbraco.Helpers](../../Reference/Umbraco.Helpers.md)


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



	