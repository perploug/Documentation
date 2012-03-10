# Installing Umbraco with WebMatrix
Follow these simple steps to be up and running with WebMatrix quickly and easily. The benefit of using WebMatrix is that it is super simple to get up and running.

1. Go to http://www.microsoft.com/web/webmatrix/ and download WebMatrix for Free
1. Once installed launch WebMatrix and from the four options shown - choose the option labelled **Site From Web Gallery**
![Web Matrix - Choose Site From Web Gallery](images/WebMatrix/webmatrix-start.png)
1. In the search box in the top right hand corner, type **Umbraco**
1. From the results select the item marked **Umbraco CMS 5**
![Web Matrix - Search for Umbraco CMS 5](images/WebMatrix/webmatrix-search.png)
1. In the **Site Name** box give your site an appropriate name and click **Next**
1. Accept the license in the next step by clicking **I Accept**<br/>
*The list of items may be more than just Umbraco as shown in the screenshot. This is because WebMatrix uses the Web Platform installer behind the scenes to install Umbraco and will also download any other software that is required in order for the website to run.*
1. WebMatrix will download the required files to run Umbraco, once done you will be prompted with a confirmation message.
1. Once completed you need to launch the website from WebMatrix.
	1. Click the Sites section in the lower left hand corner
	1. Then click on the URL such as `http://localhost:22830`<br/>*Port number may be different as it is random*
	1. The Web installer for Umbraco will now launch inside your default browser
	
## Umbraco Web Installer
This section continues from where we left off but covers the installation and configuration of Umbraco inside your web browser, when you run Umbraco for the first time.

1. You will see the welcome screen. After reading through the page click **Lets get started!**
1. On the next screen you are presented with the database options for using Umbraco with three different options. Choose the option marked **I want to use SQL CE 4** and click **Install**. This is the easiest approach and is ideal for small sites and uses a file stored on disk as the database and is the quickest option to get started with Umbraco. *If you need to migrate your database from SQL CE to SQL Server you can do at a later point.*
1. You will be shown the progress bar of the database installation, once done click **Next**
1. On the next screen you need to fill in the form to create a user so you can access the back office of Umbraco. Once completed click **Create user**
1.  From this next step you can decide if you want to install a starter kit or not. A starter kit installs an example site for you and allows you to pull it apart and learn how Umbraco works.
1. After deciding wether to skip or install a starter kit you are now finished!
1. Now click the **Set up your new website** to be logged into the Umbraco back-office. 
1. Celebrate - you're all done! 

### Congrats you have installed an Umbraco site. That wasn't so hard was it?

### Note
*You can log into your Umbraco site by entering the following into your browser - `http://yoursite.com/umbraco/`*


	