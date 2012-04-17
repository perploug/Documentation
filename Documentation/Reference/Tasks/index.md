#Tasks
Tasks in Umbraco are analogous to .NET Events, but with a little more flexibility

##Subscribing to a task


##Creating your own task

##TaskTriggers
Umbraco 5 comes with a collection of Task-triggers which you can subscribe to out of the box

###Core
<table class="grid">
	<tr>
		<th>Trigger</th>
		<th>Eventargs</th>
		<th>Raised</th>
	</tr>
	<tbody>
		<tr>
			<td>TaskTriggers.GlobalError</td>
			<td>TaskEventArgs</td>
			<td>When an unhandled exception is raised and caught by the global.asax error handler</td>
		</tr>

		<tr>
			<td>TaskTriggers.ModelPreSendToView</td>
			<td>ModelEventArgs</td>
			<td>During OnActionExecuted</td>
		</tr>

		<tr>
			<td>TaskTriggers.PostAppStartup</td>
			<td>TaskEventArgs</td>
			<td>When an unhandled exception is raised and caught by the global.asax error handler</td>
		</tr>

		<tr>
			<td>TaskTriggers.PostPackageInstall</td>
			<td>PackageInstallEventArgs</td>
			<td>Just after a package installation starts</td>
		</tr>

		<tr>
			<td>TaskTriggers.PostPackageUninstall</td>
			<td>PackageInstallEventArgs</td>
			<td>Just after a package un-installation starts</td>
		</tr>

		<tr>
			<td>TaskTriggers.PreAppStartupComplete</td>
			<td>TaskEventArgs</td>
			<td>When an unhandled exception is raised and caught by the global.asax error handler</td>
		</tr>

		<tr>
			<td>TaskTriggers.PrePackageUninstall</td>
			<td>PackageInstallEventArgs</td>
			<td>Just after a package un-installation starts</td>
		</tr>
	</tbody>
</table>









###Hive

###Relations


###Revisions
