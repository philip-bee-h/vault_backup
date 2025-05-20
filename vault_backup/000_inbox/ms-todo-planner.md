https://www.reddit.com/r/gtd/s/agmKKqnRWs

The new planner from Microsoft

Anybody else loving the personal plan option in the new planner? Together with to do and outlook it’s become perfect for GTD.

Implementing the GTD method using Microsoft To Do and the new Microsoft Planner can be very effective for personal planning. Here’s a step-by-step guide to integrate these tools seamlessly:

### Step 1: **Capture Everything with Microsoft To Do**
1. **Microsoft To Do** serves as your inbox. Capture all tasks, ideas, commitments, and random thoughts here. Use the **My Day** or **Tasks** list to jot down everything.
2. **Create Lists**: Organize your tasks into different lists such as Personal, Work, Errands, etc.
3. **Quick Add**: Use the quick add feature or voice input to capture tasks on the go.

### Step 2: **Clarify and Organize with Microsoft Planner**
1. **Clarify Tasks**: Move actionable tasks from To Do to Planner for further processing. Determine the next steps for each task.
2. **Create Buckets in Planner**:
   - **Next Actions**: Tasks you need to do next.
   - **Projects**: Larger tasks that require multiple steps.
   - **Waiting For**: Tasks that are dependent on someone else.
   - **Someday/Maybe**: Ideas or tasks for future consideration.
   - **Reference**: Information you need to keep but don't need to act on.

### Step 3: **Prioritize and Categorize**
1. **Use Labels and Priorities**: In Planner, assign labels and priorities to each task to indicate urgency and importance.
2. **Set Due Dates**: Assign deadlines to tasks to keep track of time-sensitive items.

### Step 4: **Review Regularly**
1. **Daily Review**: Check Microsoft To Do daily to capture new tasks.
2. **Weekly Review**: Use Planner to review all tasks, update statuses, and move tasks between buckets as needed.

### Step 5: **Engage and Execute**
1. **Focus on Next Actions**: Start working on tasks in the Next Actions bucket in Planner.
2. **Track Progress**: Update task status as you complete them.

### Additional Tips:
- **Integration**: Use **Microsoft Power Automate** to automate the transfer of tasks from To Do to Planner.
- **Notifications**: Enable notifications in both apps to stay on top of your tasks and deadlines.
- **Mobile Access**: Use the mobile apps for To Do

By integrating Microsoft To Do and Planner in this way, you can effectively capture, clarify, organize, and execute your tasks following the GTD methodology.


_______________________________________
AUTOMATE



here’s a step-by-step guide to moving tasks between Microsoft To Do and the new Microsoft Planner using Power Automate.

### Step 1: **Create a New Flow in Power Automate**
1. **Open Power Automate**: Go to the Power Automate website and sign in.
2. **Create a Flow**: Click on "Create" and select "Automated flow."
3. **Name Your Flow**: Give your flow a name, such as "Move Tasks from To Do to Planner."

### Step 2: **Set Up the Trigger**
1. **Choose a Trigger**: Select the trigger that will initiate the flow. For example, you can use the "When a task is added" trigger for To Do.
2. **Select the To Do List**: Choose the specific list in Microsoft To Do where your tasks are stored.

### Step 3: **Get Task Details**
1. **Add an Action**: After the trigger, add an action to get the details of the created task.
2. **Select the To Do Connector**: Choose the "Microsoft To Do" connector and the "Get task details" action.
3. **Configure the Action**: Select the task list and task ID to get the task details.

### Step 4: **Move Task to Planner**
1. **Add Another Action**: Add a new action to move the task to Planner.
2. **Select the Planner Connector**: Choose the "Microsoft Planner" connector and the "Create a task" action.
3. **Configure the Action**: Fill in the details such as the plan name, bucket (Next Actions, Projects, etc.), task title, and description.

### Step 5: **Test and Save Your Flow**
1. **Test the Flow**: Save your flow and test it by creating a task in To Do and checking if it moves to Planner.
2. **Adjust as Needed**: Make any necessary adjustments to ensure the flow works correctly.

### Example Flow:
```plaintext
Trigger: When a task is created in To Do
Action: Get task details from To Do
Action: Create a task in Planner
```

This flow will automatically move tasks from your To Do list to the appropriate bucket in Planner, helping you stay organized and efficient.
