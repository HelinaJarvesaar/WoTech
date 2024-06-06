```java
/* main.java -> 
we are getting the user input
we are sending this input to taskManager
show information to the user

taskManager.java -> 
we can add tasks to the list
we can mark them as done
and we can get the list

task.java -> 
template for a task (name, description, date, itIsDone)
*/

import java.util.ArrayList;

public class TaskManager{
  //MVP: Minimum Viable Product
  //Be able to add a Task
  //Have a list of all the tasks
  //Set a task as done, by Task name
  public ArrayList<Task> tasks = new ArrayList<Task>();

  //Because we want to use the default constructor
  //new TaskManager() - we don't need to add a custom constructor
  
  public void addTask(Task task){
    tasks.add(task);
  }

  public ArrayList<Task> getTasks(){
    return tasks;
  }
  
  //BONUS:
  //Get a random quote to stop procrastinating
  //A list of undone tasks - filter
  //A list of done tasks - filter
}



public class Task{
  public String name;
  public String description;
  public boolean isDone;

  // var task = new Task(name, description);
  public Task(String name, String description){
    this.name = name;
    this.description = description;
  }
}





import java.util.Scanner;

public class Main { //Is the UI - frontend

    public static TaskManager taskManager = new TaskManager();

    public static void main(String[] args) {
        createTask();
        showToDoList();
        //Create a menu:
        //Put it in a while loop
        //Use scanner
        //if the user presses 1, we call CreateTask
        //if the user presses 2, we call showTodoList
        //if the user presses 3, mark a task with isDone = true
        //if the user presses x, close the loop
    }

    public static void createTask(){
        Scanner scanner = new Scanner(System.in);
        // Input the task name
        System.out.println("Please input the name of the task");
        var name = scanner.nextLine();
        // Input the task description
        System.out.println("Please input the description of the task");
        var description = scanner.nextLine();

        var task = new Task(name, description);
        taskManager.addTask(task);
    }
  
    public static void showToDoList(){
        var tasks = taskManager.getTasks();
        for(var task : tasks){
          System.out.println("This is a TASK: ");
          System.out.println(task.name);
          System.out.println(task.description);
          System.out.println(task.isDone);
        } 
    }
}
```
