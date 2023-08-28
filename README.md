# Infoaidtech-Todo-list-app-
Infoaidtech-Todo-list-app by Saif Ali Khan

import tkinter as tk
from tkinter import messagebox

class ToDoListApp:
    def __init__(self, root):
        self.root = root
        self.root.title("To-Do List App")

        self.tasks = []

        self.task_entry = tk.Entry(root, width=100)
        self.task_entry.pack(pady=10)

        self.add_button = tk.Button(root, text="Add Task", command=self.add_task)
        self.add_button.pack()

        self.task_listbox = tk.Listbox(root, width=100)
        self.task_listbox.pack(pady=10)

        self.delete_button = tk.Button(root, text="Delete Task", command=self.delete_task)
        self.delete_button.pack()

        self.save_button = tk.Button(root, text="Save Tasks", command=self.save_tasks)
        self.save_button.pack()

        self.load_button = tk.Button(root, text="Load Tasks", command=self.load_tasks)
        self.load_button.pack()

        self.find_entry = tk.Entry(root, width=40)
        self.find_entry.pack(pady=10)

        self.find_button = tk.Button(root, text="Find Tasks", command=self.find_tasks)
        self.find_button.pack()

    def add_task(self):
        task = self.task_entry.get()
        if task:
            self.tasks.append(task)
            self.update_task_listbox()
            self.task_entry.delete(0, tk.END)

    def delete_task(self):
        selected_index = self.task_listbox.curselection()
        if selected_index:
            index = selected_index[0]
            del self.tasks[index]
            self.update_task_listbox()

    def update_task_listbox(self):
        self.task_listbox.delete(0, tk.END)
        for task in self.tasks:
            self.task_listbox.insert(tk.END, task)

    def save_tasks(self):
        with open("tasks.txt", "w") as file:
            for task in self.tasks:
                file.write(task + "\n")
        messagebox.showinfo("Info", "Tasks saved successfully.")

    def load_tasks(self):
        try:
            with open("tasks.txt", "r") as file:
                self.tasks = file.read().splitlines()
            self.update_task_listbox()
            messagebox.showinfo("Info", "Tasks loaded successfully.")
        except FileNotFoundError:
            messagebox.showerror("Error", "No saved tasks found.")

    def find_tasks(self):
        keyword = self.find_entry.get()
        if keyword:
            found_tasks = [task for task in self.tasks if keyword.lower() in task.lower()]
            self.update_task_listbox_with_found(found_tasks)

    def update_task_listbox_with_found(self, found_tasks):
        self.task_listbox.delete(0, tk.END)
        for task in found_tasks:
            self.task_listbox.insert(tk.END, task)

if __name__ == "__main__":
    root = tk.Tk()
    app = ToDoListApp(root)
    root.mainloop()
