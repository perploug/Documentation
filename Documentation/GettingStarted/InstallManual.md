#Installing Umbraco manually
Follow these steps to do a full manual install of Umbraco.

##Download Umbraco binaries
The stable releases of the Umbraco binaries are available from [Codeplex](http://umbraco.codeplex.com/releases). If you don't mind working with experimental builds, you could download a [nightly release](http://nightly.umbraco.org/Jupiter) at your own perril (no guarantees that it will work).

##Unzip files
Once you have the binary release of your liking saved to disk, make sure that the file has not been blocked by windows. Right-click the file you downloaded and choose "Properties". If it says at the bottom of the properties window: "This file came from another computer and might be blocked to help protect this computer" then make sure to click the "Unblock" button so that all of the binary files will be unzipped later on.

Now you can unzip the file to a location of your choosing (for example: `D:\Dev\MyUmbraco5Site\`)

##Choose your webserver
There are two ways to run the binaries in a webserver:

1. The easy way: using IIS Express
2. Internet Information Server (IIS)

###Using IIS Express - Preperation
- *Downloading IIS Express through the Web Platform installer in conjunction with Web Matrix*
- *Launching an Umbraco site through Web Matrix*

###Using IIS - Preperation
If you want to use IIS to host your Umbraco 5 site, you have two options:

1. **Using a hostname that is pointing to your local machine.**

	*Note: We strongly recommend using IIS7(.5), while IIS6 may work, this guide will cover IIS7 only. IIS7 is available in Windows 7 and Windows Server 2008.*
	
	Create a new website in IIS by right-clicking *Sites* and choosing "Add site...". The site name can be anything you want.
	
	You will then be asked what kind of application this is, make sure to pick (or create) an application pool that uses ASP.NET 4.0 as it's basis (the default ASP.NET 4.0 is configured perfectly for this, we recommend you pick that one). Use the "Select.." button to pick a different application pool, you shouldn't have to change the other settings.
	
	Finally fill in the hostname, in this example we'll use *MyUmbraco5Site.local*
	
	![Configure new website in IIS](images/manual/2012-03-12_223022.png)
	
	As a final step, you will need to add the *MyUmbraco5Site.local* hostname to your hosts file. If you haven't altered your host file before you will need to make sure that your current user has write permissions to the hosts file. The hosts file typically lives in C:\Windows\System32\drivers\etc\hosts (the file has no extension).
	
	To enable you to write to the file, right-click it and click "Properties". Go to the "Security" tab and find the current logged in user, click the user and then the "Edit..." button. Check the *Allow Modify* permission there.
	
	*Note: **this is not without risk**. If your user account can edit the hosts file, malware can do the same under your account and attempt to change the hosts file to redirect well known sites to malicious websites. Always make sure to revert the security settings if you want to be safe!*
	
	Add a line to the hosts file that points the new hostname to the local machine:
	
	`127.0.0.1 MyUmbraco5Site.local`
	
	You can now go to http://MyUmbraco5Site.local and the install wizard should appear.
	
2. **Using a virtual directory**
	
	*Note: We strongly recommend using IIS7(.5), while IIS6 may work, this guide will cover IIS7 only. IIS7 is available in Windows 7 and Windows Server 2008.*
	
	To do so, start IIS Manager and right click on the **Default Website** and choose **Add virtual directory**.
	
	![Add Virtual directory](images/manual/2012-03-12_204006.png)
	
	Fill in the virtual directory name and the path where you unzipped Umbraco's files (so the path where default.aspx and web.config are located).
	
	![Virtual directory configuration](images/manual/2012-03-12_204144.png)
	
	Now you have a new virtual directory, but IIS doesn't yet know it's an application that your pointing to, so you need to tell it that. Right-click the vdir you just created, in this case called *MyUmbraco5Site* and choose **Convert to application**.
	
	![Convert virtual directory to application](images/manual/2012-03-12_204429.png)
	
	You will then be asked what kind of application this is, make sure to pick (or create) an application pool that uses ASP.NET 4.0 as it's basis (the default ASP.NET 4.0 is configured perfectly for this, we recommend you pick that one). Use the "Select.." button to pick a different application pool, you shouldn't have to change the other settings.
	
	![Configure web application](images/manual/2012-03-12_204433.png)
	
	*Note: if ASP.NET 4.0 is not a choice in your list of application pools, don't attempt to create it manually, you will need to [register it in IIS](http://stackoverflow.com/questions/4890245/how-to-add-asp-net-4-0-as-application-pool-on-iis-7-windows-7#answer-4890368), be sure to use Brad Christie's answer here, you **have** to register it, not just create a new application pool manually, that will not work.*
	
	You can now go to http://localhost/MyUmbraco5Site and the install wizard should appear.
	


*Please note: You will not be able to run Umbraco from Visual Studio's built-in webserver Casini. You do, however, have the option to configure VS to use IIS Express or IIS if needed.*

##Choose database environment
###SQL CE
- *Easy, no config necessary*
- *Downside: doesn't scale*
- *Possible to upgrade to SQL Server (Express and higher)*
###SQL Server
- *Create empty database*
- *Create credentials*
- *Use info in install wizard*