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



###Calling the task manager from code
In a default Umbraco 5 CMS application, a reference to the default `ApplicationTaskManager` is available from any reference to the `IFrameworkContext` singleton, for example:

#####From a backoffice controller
`BackofficeRequestContext.Application.FrameworkContext.TaskManager`

#####From a regular controller