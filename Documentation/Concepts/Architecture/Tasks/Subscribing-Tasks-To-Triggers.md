#Subscribing a Task to a Task Trigger
**Topic not yet complete**

An `ApplicationTaskManager` instance needs to be told that a `Task` would like to subscribe to one or more triggers.

`ApplicationTaskManager` has several methods for this purpose:

	public ApplicationTaskManager AddTask(Lazy<AbstractTask, TaskMetadata> task)

	public ApplicationTaskManager AddTask(string triggerName, Func<AbstractTask> valueFactory, bool continueOnError)

	public ApplicationTaskManager AddTask(Func<AbstractTask> valueFactory, TaskMetadata metaData)

	public ApplicationTaskManager AddDelegateTask(string triggerName, Action<TaskExecutionContext> callback)