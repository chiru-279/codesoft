import json

# File to store tasks
TASKS_FILE = "tasks.json"

# Load tasks from file
def load_tasks():
    try:
        with open(TASKS_FILE, "r") as file:
            return json.load(file)
    except FileNotFoundError:
        return []

# Save tasks to file
def save_tasks(tasks):
    with open(TASKS_FILE, "w") as file:
        json.dump(tasks, file, indent=4)

# Add a new task
def add_task(task):
    tasks = load_tasks()
    tasks.append({"task": task, "completed": False})
    save_tasks(tasks)
    print(f"Task added: {task}")

# View all tasks
def view_tasks():
    tasks = load_tasks()
    if not tasks:
        print("No tasks found.")
    else:
        for i, task in enumerate(tasks, start=1):
            status = "✔" if task["completed"] else "✘"
            print(f"{i}. [{status}] {task['task']}")

# Mark a task as completed
def complete_task(task_number):
    tasks = load_tasks()
    if 1 <= task_number <= len(tasks):
        tasks[task_number - 1]["completed"] = True
        save_tasks(tasks)
        print("Task marked as completed.")
    else:
        print("Invalid task number.")

# Update a task
def update_task(task_number, new_task):
    tasks = load_tasks()
    if 1 <= task_number <= len(tasks):
        tasks[task_number - 1]["task"] = new_task
        save_tasks(tasks)
        print("Task updated successfully.")
    else:
        print("Invalid task number.")

# Delete a task
def delete_task(task_number):
    tasks = load_tasks()
    if 1 <= task_number <= len(tasks):
        removed_task = tasks.pop(task_number - 1)
        save_tasks(tasks)
        print(f"Deleted task: {removed_task['task']}")
    else:
        print("Invalid task number.")

# Main menu
def main():
    while True:
        print("\n--- To-Do List ---")
        print("1. Add Task")
        print("2. View Tasks")
        print("3. Mark Task as Completed")
        print("4. Update Task")
        print("5. Delete Task")
        print("6. Exit")

        choice = input("Enter your choice: ")

        if choice == "1":
            task = input("Enter task: ")
            add_task(task)
        elif choice == "2":
            view_tasks()
        elif choice == "3":
            view_tasks()
            task_number = int(input("Enter task number to mark as completed: "))
            complete_task(task_number)
        elif choice == "4":
            view_tasks()
            task_number = int(input("Enter task number to update: "))
            new_task = input("Enter new task: ")
            update_task(task_number, new_task)
        elif choice == "5":
            view_tasks()
            task_number = int(input("Enter task number to delete: "))
            delete_task(task_number)
        elif choice == "6":
            print("Exiting To-Do List. Goodbye!")
            break
        else:
            print("Invalid choice! Please select a valid option.")

if __name__ == "__main__":
    main()
