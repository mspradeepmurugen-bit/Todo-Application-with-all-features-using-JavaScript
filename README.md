# Todo-Application-with-all-features-using-JavaScript
# Ex03 To-Do List using JavaScript
## Date:17/11/2026

## AIM
To create a To-do Application with all features using JavaScript.

## ALGORITHM
### STEP 1
Build the HTML structure (index.html).

### STEP 2
Style the App (style.css).

### STEP 3
Plan the features the To-Do App should have.

### STEP 4
Create a To-do application using Javascript.

### STEP 5
Add functionalities.

### STEP 6
Test the App.

### STEP 7
Open the HTML file in a browser to check layout and functionality.

### STEP 8
Fix styling issues and refine content placement.

### STEP 9
Deploy the website.

### STEP 10
Upload to GitHub Pages for free hosting.

## PROGRAM
```

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Todo Application</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>

    <div class="container">
        <header>
            <h1>✓ Todo App</h1>
            <p>Plan • Organize • Get Things Done</p>
        </header>

        <div class="dashboard">
            <div class="card">
                <h2 id="totalTasks">0</h2>
                <p>Total Tasks</p>
            </div>

            <div class="card">
                <h2 id="completedTasks">0</h2>
                <p>Completed</p>
            </div>

            <div class="card">
                <h2 id="pendingTasks">0</h2>
                <p>Pending</p>
            </div>
        </div>

        <div class="task-input">
            <input type="text" id="taskInput"
                placeholder="Add a new task...">
            <button id="addBtn">+ Add Task</button>
        </div>

        <div class="task-list">
            <h2>My Tasks</h2>
            <ul id="taskList"></ul>
        </div>
    </div>

    <script src="script.js"></script>
</body>
</html>


* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: Arial, sans-serif;
}

body {
    background: #f1f0ff;
    min-height: 100vh;
    padding: 30px;
}

.container {
    max-width: 1000px;
    margin: auto;
    background: white;
    padding: 30px;
    border-radius: 20px;
    box-shadow: 0 5px 25px rgba(0,0,0,0.08);
}

header {
    margin-bottom: 30px;
}

header h1 {
    font-size: 36px;
    color: #5635e8;
}

header p {
    color: #777;
    margin-top: 8px;
}

.dashboard {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 20px;
    margin-bottom: 30px;
}

.card {
    background: #eeeaff;
    padding: 25px;
    border-radius: 12px;
    text-align: center;
}

.card h2 {
    font-size: 30px;
    color: #5635e8;
}

.card p {
    margin-top: 8px;
    color: #555;
}

.task-input {
    display: flex;
    gap: 10px;
    margin-bottom: 30px;
}

.task-input input {
    flex: 1;
    padding: 15px;
    border: 1px solid #ccc;
    border-radius: 10px;
    font-size: 16px;
}

button {
    background: #5635e8;
    color: white;
    border: none;
    padding: 15px 22px;
    border-radius: 10px;
    cursor: pointer;
}

button:hover {
    background: #4021c5;
}

.task-list h2 {
    margin-bottom: 15px;
}

#taskList {
    list-style: none;
}

#taskList li {
    background: #f8f7ff;
    padding: 15px;
    margin-bottom: 10px;
    border-radius: 10px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 10px;
}

@media (max-width: 600px) {
    body {
        padding: 10px;
    }

    .dashboard {
        grid-template-columns: 1fr;
    }

    .task-input {
        flex-direction: column;
    }
}


let tasks = JSON.parse(localStorage.getItem("tasks")) || [];

const taskInput = document.getElementById("taskInput");
const addBtn = document.getElementById("addBtn");
const taskList = document.getElementById("taskList");

function saveTasks() {
    localStorage.setItem("tasks", JSON.stringify(tasks));
}

function addTask() {
    const taskName = taskInput.value.trim();

    if (taskName === "") {
        alert("Please enter a task!");
        return;
    }

    tasks.push({
        id: Date.now(),
        name: taskName,
        completed: false
    });

    taskInput.value = "";
    saveTasks();
    displayTasks();
}

function displayTasks() {
    taskList.innerHTML = "";

    tasks.forEach(task => {
        const li = document.createElement("li");

        const span = document.createElement("span");
        span.textContent = task.name;

        if (task.completed) {
            span.style.textDecoration = "line-through";
            span.style.color = "gray";
        }

        const actions = document.createElement("div");

        const completeBtn = document.createElement("button");
        completeBtn.textContent = task.completed ? "Undo" : "Done";
        completeBtn.onclick = () => toggleTask(task.id);

        const editBtn = document.createElement("button");
        editBtn.textContent = "Edit";
        editBtn.onclick = () => editTask(task.id);

        const deleteBtn = document.createElement("button");
        deleteBtn.textContent = "Delete";
        deleteBtn.style.background = "#e53935";
        deleteBtn.onclick = () => deleteTask(task.id);

        actions.append(completeBtn, editBtn, deleteBtn);
        li.append(span, actions);
        taskList.appendChild(li);
    });

    updateDashboard();
}

function toggleTask(id) {
    tasks = tasks.map(task =>
        task.id === id
            ? { ...task, completed: !task.completed }
            : task
    );

    saveTasks();
    displayTasks();
}

function editTask(id) {
    const task = tasks.find(task => task.id === id);
    const newName = prompt("Edit your task:", task.name);

    if (newName !== null && newName.trim() !== "") {
        task.name = newName.trim();
        saveTasks();
        displayTasks();
    }
}

function deleteTask(id) {
    if (confirm("Are you sure you want to delete this task?")) {
        tasks = tasks.filter(task => task.id !== id);
        saveTasks();
        displayTasks();
    }
}

function updateDashboard() {
    document.getElementById("totalTasks").textContent = tasks.length;

    document.getElementById("completedTasks").textContent =
        tasks.filter(task => task.completed).length;

    document.getElementById("pendingTasks").textContent =
        tasks.filter(task => !task.completed).length;
}

addBtn.addEventListener("click", addTask);

taskInput.addEventListener("keypress", function(event) {
    if (event.key === "Enter") {
        addTask();
    }
});

displayTasks();
```


## OUTPUT
```
OneDrive/Pictures/Screenshots/Screenshot 2026-11-17 111802.png
```

## RESULT
The program for creating To-do list using JavaScript is executed successfully.
