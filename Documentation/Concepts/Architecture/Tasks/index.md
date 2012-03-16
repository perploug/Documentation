#Tasks
*Tasks in Umbraco are analogous to .NET Events, but with a little more flexibility*

***Note:** Umbraco Tasks are not to be confused with classes of the same name from the `System.Threading.Tasks` namespace in .NET Framework 4 and above.*

##Why would I use Tasks?

-  If you want to run some code in response to an event (or *trigger*) in another part of the system
-  If you would like to be notified of some content being saved, or published in order to notify an external system, or fire off an email
-  If you would like to be notified of content changes, running your own business or workflow rules, and optionally change the content, set its revision status to something else, or cancel the action entirely.
-  If you are building your own plugin, and want to enable others to be able to respond to your own triggers using the same system.

##General overview
In Umbraco 5, a Task is a piece of functionality that the developer wants to be run when certain conditions are met in another part of the application. These conditions are known as 'task triggers'. When another part of the application fires a task trigger, all the Tasks that are registered for that trigger are executed by an `ApplicationTaskManager`.

The Umbraco 5 core collection of subsystems exposes many task triggers already, but a developer can also fire their own, so that other third-party components or the developer's own Tasks can be run in the same fashion.

The Task Manager subsystem consists of three elements:

-  Task Triggers: A `string` key that identifies the type of activity
-  `Task` entries: A module of code that constitutes the work to be done
-  An `ApplicationTaskManager` whose job it is to maintain a list of registered tasks and associated trigger keys

##Overview: Registering custom Tasks
You can register either either an entire class with an `ApplicationTaskManager`, or just a simple delegate like the traditional .NET event system, using a choice of approaches:

-  By getting a reference to an instance of `ApplicationTaskManager` and directly calling any of the Addxxx methods
-  By automatic discovery during application startup, by decorating your Task class with one or more `Umbraco.Framework.Tasks.TaskAttribute` declaring both a unique ID and a task trigger.

*For more detail, see the [Subscribing a Task to a Task Trigger](Subscribing-Tasks-To-Triggers.md) topic*

##Overview: How Tasks are executed
An `ApplicationTaskManager` keeps a reference to all registered Tasks, together with the trigger names for which those Tasks want to be executed. Callers tell the `ApplicationTaskManager` to fire a particular trigger, and the manager is responsible for then executing the matching Tasks in the order that they were registered.

The manager has a few methods for executing tasks:

*The following is most similar to a traditional .NET event signature. It takes a trigger name, a sender (usually the caller), and some event arguments that will be made available to the executing tasks. The method returns a `TaskExecutionContext` which provides the caller with information about whether the task completed, or any exceptions that were raised, etc.*

	public TaskExecutionContext ExecuteInContext(string triggerName, object sender, TaskEventArgs eventArgs)

*These accept an existing instance of a `TaskExecutionContext` which is useful if you would like to fire multiple triggers within the same 'scope'. To make it easier to chain method calls together, these return the same instance of `ApplicationTaskManager`.*

	public ApplicationTaskManager ExecuteInContext(string triggerName, TaskExecutionContext context)

	public ApplicationTaskManager ExecuteInContext(string triggerName, TaskExecutionContext context, out bool canceled)

###Firing a task trigger from code
In a default Umbraco 5 CMS application, a reference to the default `ApplicationTaskManager` is available from any reference to the `IFrameworkContext` singleton, for example:

####Obtaining a reference to the task manager

*From a backoffice controller*: If you've inherited from any of the standard abstract controllers for the Umbraco 5 backoffice, you will have a 'BackofficeRequestContext' property that allows access ultimately to an `IFrameworkContext`:

	BackofficeRequestContext.Application.FrameworkContext.TaskManager

*From a regular controller*: You can either add a constructor parameter to have Umbraco's IoC container inject the context for you, or use MVC's `DependencyResolver`. 

####Example
In the below, the method `MyAction` refers to the injected context, and the static method `RaiseATrigger` shows how to get a reference to an `IFrameworkContext` when you can't have a service injected, for example in a static method.

    using Umbraco.Framework.Context;
    using Umbraco.Framework.Tasks;

    public class ExampleController : Controller
    {
        private readonly IFrameworkContext _frameworkContext;

        public ExampleController(IFrameworkContext frameworkContext)
        {
            // Because ExampleController is created by a controller factory that uses
            // the dependency injection service configured in Umbraco 5, the
            // frameworkContext parameter will be injected automatically.
            _frameworkContext = frameworkContext;
        }

        public ActionResult MyAction()
        {
            // Here we're going to tell the TaskManager to execute any tasks that subscribe to the
            // 'pre-myAction-executes' trigger
            _frameworkContext.TaskManager.ExecuteInContext("pre-myAction-executes", this, new MyCustomEventArgs());
            return View();
        }

        public static void RaiseATrigger()
        {
            // Because this method is static, we can use the DependencyResolver to get an instance of anything that
            // has an IFrameworkContext in order to call the task manager
            var frameworkContext = DependencyResolver.Current.GetService<IFrameworkContext>();

            // After that, firing a trigger is similar, but because this is a static method, there's no instance of 
            // "us" as a caller in order to pass to the task. So let's make our own TaskExecutionContext to describe
            // the scope.
            var context = new TaskExecutionContext();
            context.EventArgs = new MyCustomEventArgs();
            frameworkContext.TaskManager.ExecuteInContext("my-custom-trigger", context);
        }
    }