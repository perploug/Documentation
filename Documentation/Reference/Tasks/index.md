#Tasks
Tasks in Umbraco are analogous to .NET Events, but with a little more flexibility

**WORK IN PROGRESS**

##Subscribing a task to a trigger
Subscribing to any trigger from the Umbraco task manager, requires setting up a specific task to handle every time the trigger fires. Some triggers enables the task to cancel the pending action, this triggers are always prefixed with "pre".


###usings
The namespace references required for the below samples

	//for task executioncontext
	using Umbraco.Framework.Tasks;
	//for task triggers
	using Umbraco.Framework;
	//For abstractwebtask
	using Umbraco.Cms.Web.Tasks;
	//for IUmbracoApplicationContext
	using Umbraco.Cms.Web.Context;
	//use diagnostics for logging
	using Umbraco.Framework.Diagnostics;

###Task sample
Setting up the task, the sample below subscribes to a trigger which fires every time a revision is added or updated, and requires a unique GUID in its Task attribute to be registered correctly.

	[Task("{C43B3112-6335-4818-AD4D-0714D1359683}", TaskTriggers.Hive.Revisions.PostAddOrUpdate)]
    public class RunOnAddOrUpdate : AbstractWebTask
    {
    	//the constructor gets the Umbraco Application Context injected
        public RunOnAddOrUpdate(IUmbracoApplicationContext applicationContext) : base(applicationContext){}

        //Method which is executed when trigger fires, this receives a Task context
        public override void Execute(TaskExecutionContext context)
        {
            var item = context.EventSource;

            //In this case we cast the CallerEventArgs to HiveRevisionPostActionEventArgs
            var args = (Umbraco.Hive.HiveRevisionPostActionEventArgs)context.EventArgs.CallerEventArgs;

            //Finally we log the execution
            LogHelper.Warn<RunOnAddOrUpdate>("This ID was just added or updated" + args.Entity.Item.Id.ToString());
        }
    }


###Cancelable Task sample
Some triggers have the ability to cancel the action about to be executed. These are cancellable triggers, and are prefixed with "pre", like in the below sample using Revisions.PreAddOrUpdate

	[Task("{C43B3112-6335-4818-AD4D-0714D1359693}", TaskTriggers.Hive.Revisions.PreAddOrUpdate)]
    public class RunOnAddOrUpdate : AbstractWebTask
    {
        public RunOnAddOrUpdate(IUmbracoApplicationContext applicationContext) : base(applicationContext) { }

        public override void Execute(TaskExecutionContext context)
        {
            //cancel the revision being added or updated
            context.Cancel = true;
        }
    }	


##Creating your own trigger
The umbraco 5 taskmanager enables you to create any trigger of your own, which you can set to run at any given time, enabling other parts of the system to perform actions when your custom trigger is fired. You have the ability to trigger both pre or post triggers.

A trigger is nothing but a unique name, which for consistancy should be stored as a const string.

###Defining a trigger name
Define unique trigger names, you should follow naming conventions as below

	public const string PreCommentMarkedAsSpam = "pluginname-pre-comment-marked-as-spam";
    public const string PostCommentMarkedAsSpam = "pluginname-post-comment-marked-as-spam";

###Accessing the TaskManager
Now that we have a name, we can fire the trigger anywhere in our code, as long as we have acces to IFrameworkContext, which holds the TaskManager

In a SurfaceController we have IUmbracoApplicationContext which is injected by the controller factory

	public MySurfaceController(IUmbracoApplicationContext context)
    {
       var manager = context.FrameworkContext.TaskManager;
    }

Same goes with a Controller which can have IFrameworkContext injected
	
	public ExampleController(IFrameworkContext frameworkContext)
        {
            // Because ExampleController is created by a controller factory that uses
            // the dependency injection service configured in Umbraco 5, the
            // frameworkContext parameter will be injected automatically.
            var manager = frameworkContext.TaskManager;
        }

Or it can be accessed by MVCs standard DependencyResolver

	var frameworkContext = DependencyResolver.Current.GetService<IFrameworkContext>();
	var manager = frameworkContext.TaskManager;

###Executing tasks
Now that we access to the task manager, we can trigger any task that subscribes to custom triggers we defined above

	var manager = context.FrameworkContext.TaskManager;

	//Create and populate a context for the task
	var taskContext = new TaskExecutionContext();
    taskContext.EventSource = this;
    taskContext.EventArgs = new MyCustomEventArgs();
    
    //execute
    manager.ExecuteInContext("MyTriggerName", taskContext);

    //execute and return if action should cancelled or not
    bool cancelAction = false;
    manager.ExecuteInContext("MyTriggerName", taskContext, out cancelAction);



##TaskTriggers
Umbraco 5 comes with a collection of Task-triggers which you can subscribe to out of the box

Each Trigger returns a distinct context, containing a CallerEventargs, which will contain data relavant to the trigger.

All build-in triggers are located in the Umbraco.FrameWork.TaskTrggers class

###Core
<table class="grid">
	<tr>
		<th>Trigger</th>
		<th>CallerEventargs</th>
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
			<td>null</td>
			<td>After application startup is completed</td>
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
			<td>null</td>
			<td>Just after initial system boot, but before umbraco application starts</td>
		</tr>

		<tr>
			<td>TaskTriggers.PrePackageUninstall</td>
			<td>PackageInstallEventArgs</td>
			<td>Just after a package un-installation starts</td>
		</tr>
	</tbody>
</table>

###Hive
<table class="grid">
	<tr>
		<th>Trigger</th>
		<th>CallerEventargs</th>
		<th>Raised</th>
	</tr>
	<tbody>
		<tr>
			<td>TaskTriggers.Hive.PreAddOrUpdateOnUnitComplete</td>
			<td>HiveEntityPreActionEventArgs</td>
			<td>At the beginning of a work unit completion, triggered for each entity set to be added or updated, this trigger can cancel the entire unit of work</td>
		</tr>
		<tr>
			<td>TaskTriggers.Hive.PostAddOrUpdateOnUnitComplete</td>
			<td>HiveEntityPostActionEventArgs</td>
			<td>At the end of a work unit completion, triggered for each entity added or updated</td>
		</tr>

		<tr>
			<td>TaskTriggers.Hive.PostReadEntity</td>
			<td>HiveEntityPostActionEventArgs</td>
			<td>When a entity is read from Hive and considered ready</td>
		</tr>
		<tr>
			<td>TaskTriggers.Hive.PostQueryResultsAvailable</td>
			<td>HiveQueryResultEventArgs</td>
			<td>When a ExecuteSingle, ExecuteScalar or ExecuteMany Query result is returned from Hive</td>
		</tr>
		<tr>
			<td>TaskTriggers.Hive.PostInstall</td>
			<td>EventArgs</td>
			<td>When a Hive Provider installation is completed successfully</td>
		</tr>
		<tr>
			<td>TaskTriggers.Hive.InstallStatusChanged</td>
			<td>N/A</td>
			<td>When a hive provider changes its install status, currently not active</td>
		</tr>

		<tr>
			<td>TaskTriggers.Hive.PreAddOrUpdate</td>
			<td>HiveEntityPreActionEventArgs</td>
			<td>Before a entity is added to a unit of work, for either creation or updating, this task can cancel the add</td>
		</tr>
		<tr>
			<td>TaskTriggers.Hive.PostAddOrUpdate</td>
			<td>HiveEntityPostActionEventArgs</td>
			<td>After a entity is added to a unit of work, for either creation or updating</td>
		</tr>

		<tr>
			<td>TaskTriggers.Hive.PreDelete</td>
			<td>TaskEventArgs</td>
			<td>Before a entity is added to a unit of work, for deletion, this task can cancel the add</td>
		</tr>

		<tr>
			<td>TaskTriggers.Hive.PostDelete</td>
			<td>HiveEntityPostDeletionEventArgs</td>
			<td>After a entity is added to a unit of work, for deletion</td>
		</tr>

		<tr>
			<td>TaskTriggers.Hive.PreDeleteOnUnitComplete</td>
			<td>HiveEntityPreDeletionEventArgs</td>
			<td>At the beginning of a work unit completion, triggered for each entity set to be deleted, this trigger can cancel the entire unit of work</td>
		</tr>

		<tr>
			<td>TaskTriggers.Hive.PostDeleteOnUnitComplete</td>
			<td>HiveEntityPostDeletionEventArgs</td>
			<td>At the end of a work unit completion, triggered for each entity deleted</td>
		</tr>	
	</tbody>
</table>

###Revisions
<table class="grid">
	<tr>
		<th>Trigger</th>
		<th>CallerEventargs</th>
		<th>Raised</th>
	</tr>
	<tbody>
		<tr>
			<td>TaskTriggers.Hive.Revisions.PreAddOrUpdateOnUnitComplete</td>
			<td>HiveRevisionPreActionEventArgs</td>
			<td>At the beginning of a work unit completion, triggered for each entity revision set to be added or updated, this trigger can cancel the entire unit of work</td>
		</tr>
		<tr>
			<td>TaskTriggers.Hive.Revisions.PostAddOrUpdateOnUnitComplete</td>
			<td>HiveRevisionPostActionEventArgs</td>
			<td>At the end of a work unit completion, triggered for each entity added or updated</td>
		</tr>

		<tr>
			<td>TaskTriggers.Hive.Revisions.PreAddOrUpdate</td>
			<td>HiveRevisionPreActionEventArgs</td>
			<td>Before a entity revision is added to a unit of work, for either creation or updating, this task can cancel the add</td>
		</tr>
		<tr>
			<td>TaskTriggers.Hive.Revisions.PostAddOrUpdate</td>
			<td>HiveRevisionPostActionEventArgs</td>
			<td>After a entity revision is added to a unit of work, for either creation or updating</td>
		</tr>
	</tbody>
</table>

###Relations
<table class="grid">
	<tr>
		<th>Trigger</th>
		<th>CallerEventargs</th>
		<th>Raised</th>
	</tr>
	<tbody>
		<tr>
			<td>TaskTriggers.Hive.Relations.PreRelationAdded</td>
			<td>HiveRelationPreActionEventArgs</td>
			<td>before a relation is added to an entity, this can cancell the add</td>
		</tr>
		<tr>
			<td>TaskTriggers.Hive.Relations.PostRelationAdded</td>
			<td>HiveRelationPostActionEventArgs</td>
			<td>after a relation is added to an entity</td>
		</tr>
		<tr>
			<td>TaskTriggers.Hive.Relations.PreRelationRemoved</td>
			<td>HiveRelationByIdPreActionEventArgs</td>
			<td>before a relation is removed from an entity, this can cancel the removal</td>
		</tr>
		<tr>
			<td>TaskTriggers.Hive.Relations.PostRelationRemoved</td>
			<td>HiveRelationByIdPostActionEventArgs</td>
			<td>after a relation is removed from an entity</td>
		</tr>
	</tbody>
</table>
