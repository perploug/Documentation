#Tasks
*Tasks in Umbraco are analogous to .NET Events, but with a little more flexibility*
**Note:** Umbraco Tasks are not to be confused with classes of the same name from the `System.Threading.Tasks` namespace in .NET Framework 4 and above.

##What are Tasks
In Umbraco 5, a Task is a piece of functionality that the developer wants to be run when certain conditions are met in another part of the application. These conditions are known as 'task triggers'. When another part of the application fires a task trigger, all the Tasks that are registered for that trigger are executed by an `ApplicationTaskManager`.

The Umbraco 5 core collection of subsystems exposes many task triggers already, but a developer can also fire their own, so that other third-party components or the developer's own Tasks can be run in the same fashion.

The Task Manager subsystem consists of three elements:

-  Task Triggers: A `string` key that identifies the type of activity
-  `Task` entries: A module of code that constitutes the work to be done
-  An `ApplicationTaskManager` whose job it is to maintain a list of registered tasks and associated trigger keys

##How Tasks are executed
