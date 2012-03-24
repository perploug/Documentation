# Users

## What is a User?
A User is an account that can be used to access the Umbraco backend / admin area.
This is also the main difference between Users and Members. 

#### User configuration
Users can be configured in the following ways:

- Content tree starting point. 
	- If configured, the User will only see what is underneath the selected node.
- Media tree starting point. 
	- If configured, the User will only see what is underneath the selected node.
- Section access. 
	- I.e. You can decide wether a user should only have access to the Content and Media sections.
- You can also disable login for users. 
	- I.e. Make them inactive.
- User Group membership. 
	- Every user can be a member of one or more groups. 
	- This can give you more fine-grained permission control.

## User groups
You can add as many groups you want, but the default ones are:

- Administrator.
	- Members of this group have by default all permissions.
- Anonymous. 
	- Normal visitors of your site has this permission by default.
	- The only permission this group has by default is "View", meaning they can browse your published content.

#### User group permissions
Each group can be given different permissions. 
All permissions can be set to either "Allow", "Deny" or "Inherit".

Below is a list of permissions you can assign to a User Group, and basic explanation of what they do:

- Back-Office Access
	- Permission to log into "/Umbraco/" and administer the site from there.
- Audit
	- View the audit trail for a content node.
	- Not available in v5.0.1
- Copy
	- Permission to copy a content or media node from one place to another.
- Create
	- Permission to create new content or media nodes.
- Delete
	- Permission to delete content or media nodes.
- Empty Recycle Bin
	- Permission to empty the content or media section recycle bin. 
- Hostnames
	- Set the hostname for a content node and its subnodes.
- Language
	- Set the language for a content or media node and its subnodes.
- Move
	- Permission to move a content or media node from one place to another.
- Permissions
	- Set user permissions for a content tree node and its subnodes.
- Public Access
	- Restrict public access to a content node and its subnodes.
	- Not available in v5.0.1
- Publish
	- Permission to publish a content or media node. I.e. make a node visible to visitors.
- Rollback
	- Permission to roll back changes made to a content node.
- Save
	- Permission to save a content or media node.
- Send to Publish
	- Permission to start a publish workflow. I.e. send a content node to an editor for review and publish.
	- Not available in v5.0.1
- Send to Translate
	- Permission to start a translation workflow. I.e. send a content node to a translator.
	- Not available in v5.0.1
- Sort
	- Permission to sort content or media nodes.
- Translate
	- Permission to translate content nodes.
	- Not available in v5.0.1
- Unpublish
	- Permission to unpublish content or media nodes.
- Update
	- Permission to update content nodes.
	- Not available in v5.0.1
- View
	- Permission to view content or media nodes.