# To-Do-List-App
This Python-based To-Do List app allows you to manage tasks efficiently. Add new tasks, view your pending list, and mark tasks as completed. All tasks are saved to a file, so you never lose your progress.
import os

# File to store tasks
TASK_FILE = "tasks.txt"

def load_tasks():
    """Load tasks from the file."""
    if not os.path.exists(TASK_FILE):
        return []
    with open(TASK_FILE, "r") as file:
        tasks = [line.strip().split(" | ") for line in file.readlines()]
        return [{"task": task, "completed": completed == "True"} for task, completed in tasks]

def save_tasks(tasks):
    """Save tasks to the file."""
    with open(TASK_FILE, "w") as file:
        for task in tasks:
            file.write(f"{task['task']} | {task['completed']}\n")

def display_tasks(tasks):
    """Display all tasks."""
    if not tasks:
        print("\nNo tasks found.")
        return

    print("\nTo-Do List:")
    for i, task in enumerate(tasks, start=1):
        status = "✔" if task["completed"] else "❌"
        print(f"{i}. {task['task']} [{status}]")

def add_task(tasks):
    """Add a new task."""
    task_name = input("\nEnter the task: ").strip()
    tasks.append({"task": task_name, "completed": False})
    print(f"Task '{task_name}' added!")

def complete_task(tasks):
    """Mark a task as completed."""
    display_tasks(tasks)
    try:
        task_num = int(input("\nEnter the task number to mark as completed: "))
        if 1 <= task_num <= len(tasks):
            tasks[task_num - 1]["completed"] = True
            print(f"Task '{tasks[task_num - 1]['task']}' marked as completed!")
        else:
            print("Invalid task number.")
    except ValueError:
        print("Please enter a valid number.")

def main():
    """Main function to run the To-Do List app."""
    tasks = load_tasks()
    while True:
        print("\nTo-Do List App")
        print("1. View Tasks")
        print("2. Add Task")
        print("3. Complete Task")
        print("4. Exit")

        choice = input("Choose an option: ").strip()
        if choice == "1":
            display_tasks(tasks)
        elif choice == "2":
            add_task(tasks)
        elif choice == "3":
            complete_task(tasks)
        elif choice == "4":
            save_tasks(tasks)
            print("Goodbye!")
            break
        else:
            print("Invalid choice. Please try again.")

if _name_ == "_main_":
    main()
