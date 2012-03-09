#Plugins and Packages

##What is a plugin?
A Plugin is a single component of functionality that extends the back-office; these include a Tree, an Editor or a Property Editor, to name a few.

##What is a package?
A Package is a collection of Plugins that have been prepared for installation into the back-office.

##Types of plugins
The following types of plugin can extend the back-office.

###Applications
An application defines a section of the back-office. A default Umbraco installation comes with several default applications - these are Content, Media, Settings, etc.

###Trees
Trees are building blocks of application sections. An application contains one or more Trees, for example the Settings section consists of several trees; e.g. the Stylesheet tree, the Document Types tree, etc.  Each Tree can contain nodes, with each node providing specific functionality.

###Menu Items
Trees (and nodes within a Tree) can have additional functionality via its (right-click) context menu. These menu items perform custom actions for the selected item.

###Editors
An Editor enables interactions in the right/main editor panel and modal dialogs in the back-office. Typically these are used for editing content, scripts or templates.

###Dashboards
A Dashboard provides a piece of functionality to enhance an application section. These usually appear as the default panel within a section.

###Property Editors
[TBC]

###Parameter Editors
[TBC]