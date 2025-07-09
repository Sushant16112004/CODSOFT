<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TaskMaster Pro</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        :root {
            --primary: #4f46e5;
            --primary-light: #6366f1;
            --secondary: #f43f5e;
            --success: #10b981;
            --warning: #f59e0b;
            --info: #3b82f6;
        }

        .priority-low {
            border-left: 4px solid #10b981;
        }
        
        .priority-medium {
            border-left: 4px solid #f59e0b;
        }
        
        .priority-high {
            border-left: 4px solid #f43f5e;
        }

        .task-completed {
            opacity: 0.7;
            background-color: #f5f5f5;
        }

        .task-completed .task-title {
            text-decoration: line-through;
            color: #6b7280;
        }

        .fade-in {
            animation: fadeIn 0.3s ease-in-out;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .date-picker::-webkit-calendar-picker-indicator {
            filter: invert(0.5);
            cursor: pointer;
        }
    </style>
</head>
<body class="bg-gray-50 min-h-screen">
    <div class="max-w-4xl mx-auto px-4 py-8">
        <!-- Header -->
        <header class="mb-8">
            <div class="flex items-center justify-between">
                <div class="flex items-center space-x-2">
                    <img src="https://storage.googleapis.com/workspace-0f70711f-8b4e-4d94-86f1-2a93ccde5887/image/829645b3-878a-457d-84b3-e73cac75e04e.png" alt="TaskMaster Pro logo - A clipboard with a checkmark, colored in indigo" class="rounded-lg">
                    <h1 class="text-2xl font-bold text-gray-800">TaskMaster Pro</h1>
                </div>
                <div class="flex items-center space-x-4">
                    <button id="clear-completed" class="text-sm text-gray-500 hover:text-gray-700">Clear Completed</button>
                    <button id="dark-mode-toggle" class="w-10 h-10 rounded-full flex items-center justify-center bg-gray-200 hover:bg-gray-300 transition-colors">
                        <img src="https://storage.googleapis.com/workspace-0f70711f-8b4e-4d94-86f1-2a93ccde5887/image/0da47c20-4400-4929-9f4f-c00f2b8901a2.png" alt="Dark mode toggle icon - Half moon and half sun symbol" class="w-5 h-5">
                    </button>
                </div>
            </div>
            <p class="text-gray-500 text-sm mt-2">Organize, prioritize, and conquer your tasks</p>
        </header>

        <!-- Task Form -->
        <div class="bg-white rounded-xl shadow-sm p-6 mb-6">
            <form id="task-form" class="space-y-4">
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div class="space-y-2">
                        <label for="title" class="block text-sm font-medium text-gray-700">Task Title*</label>
                        <input 
                            type="text" 
                            id="title" 
                            required
                            class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500"
                            placeholder="Enter task title">
                    </div>
                    <div class="space-y-2">
                        <label for="date" class="block text-sm font-medium text-gray-700">Due Date</label>
                        <input 
                            type="date" 
                            id="date"
                            class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500 date-picker">
                    </div>
                </div>
                <div class="space-y-2">
                    <label for="description" class="block text-sm font-medium text-gray-700">Description</label>
                    <textarea 
                        id="description" 
                        rows="2"
                        class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500"
                        placeholder="Add more details about the task (optional)"></textarea>
                </div>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div class="space-y-2">
                        <label class="block text-sm font-medium text-gray-700">Priority</label>
                        <div class="flex space-x-4">
                            <label class="inline-flex items-center">
                                <input type="radio" name="priority" value="low" class="text-emerald-500 focus:ring-emerald-500" checked>
                                <span class="ml-2 text-sm text-gray-700">Low</span>
                            </label>
                            <label class="inline-flex items-center">
                                <input type="radio" name="priority" value="medium" class="text-amber-500 focus:ring-amber-500">
                                <span class="ml-2 text-sm text-gray-700">Medium</span>
                            </label>
                            <label class="inline-flex items-center">
                                <input type="radio" name="priority" value="high" class="text-rose-500 focus:ring-rose-500">
                                <span class="ml-2 text-sm text-gray-700">High</span>
                            </label>
                        </div>
                    </div>
                    <div class="flex items-end justify-end space-x-3">
                        <button 
                            type="button" 
                            id="cancel-btn"
                            class="px-4 py-2 border border-gray-300 rounded-md text-sm font-medium text-gray-700 hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-indigo-500 hidden">
                            Cancel
                        </button>
                        <button 
                            type="submit" 
                            id="submit-btn"
                            class="px-4 py-2 bg-indigo-600 rounded-md text-sm font-medium text-white hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-indigo-500">
                            Add Task
                        </button>
                    </div>
                </div>
            </form>
        </div>

        <!-- Task Filters -->
        <div class="flex items-center justify-between mb-4">
            <div class="flex space-x-2 overflow-x-auto pb-2">
                <button 
                    data-filter="all"
                    class="filter-btn px-3 py-1 text-sm rounded-md bg-indigo-100 text-indigo-700">
                    All Tasks
                </button>
                <button 
                    data-filter="active"
                    class="filter-btn px-3 py-1 text-sm rounded-md bg-gray-100 text-gray-700 hover:bg-gray-200">
                    Active
                </button>
                <button 
                    data-filter="completed"
                    class="filter-btn px-3 py-1 text-sm rounded-md bg-gray-100 text-gray-700 hover:bg-gray-200">
                    Completed
                </button>
                <button 
                    data-filter="today"
                    class="filter-btn px-3 py-1 text-sm rounded-md bg-gray-100 text-gray-700 hover:bg-gray-200">
                    Due Today
                </button>
                <button 
                    data-filter="high"
                    class="filter-btn px-3 py-1 text-sm rounded-md bg-gray-100 text-gray-700 hover:bg-gray-200">
                    High Priority
                </button>
            </div>
            <div class="text-sm text-gray-500">
                <span id="task-count">0 tasks</span>
            </div>
        </div>

        <!-- Task List -->
        <div id="task-list" class="space-y-3">
            <!-- Tasks will be dynamically inserted here -->
            <div class="bg-white rounded-xl shadow-sm p-6 text-center text-gray-500" id="empty-state">
                <img src="https://storage.googleapis.com/workspace-0f70711f-8b4e-4d94-86f1-2a93ccde5887/image/4a613ccb-cfd8-432a-8d27-74e73deabc1d.png" alt="Empty task list illustration - A clipboard with no items, looking clean and organized" class="mx-auto mb-4">
                <p>No tasks yet. Add one above to get started!</p>
            </div>
        </div>
    </div>

    <!-- Task Modal -->
    <div id="task-modal" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center hidden z-50">
        <div class="bg-white rounded-xl shadow-xl w-full max-w-md mx-4">
            <div class="p-6">
                <h3 id="modal-title" class="text-lg font-medium text-gray-900 mb-4">Edit Task</h3>
                <form id="edit-form">
                    <div class="space-y-4">
                        <div>
                            <label for="edit-title" class="block text-sm font-medium text-gray-700">Title</label>
                            <input type="text" id="edit-title" class="w-full px-3 py-2 border border-gray-300 rounded-md mt-1">
                        </div>
                        <div>
                            <label for="edit-description" class="block text-sm font-medium text-gray-700">Description</label>
                            <textarea id="edit-description" rows="3" class="w-full px-3 py-2 border border-gray-300 rounded-md mt-1"></textarea>
                        </div>
                        <div>
                            <label for="edit-date" class="block text-sm font-medium text-gray-700">Due Date</label>
                            <input type="date" id="edit-date" class="w-full px-3 py-2 border border-gray-300 rounded-md mt-1 date-picker">
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-700">Priority</label>
                            <div class="flex space-x-4 mt-1">
                                <label class="inline-flex items-center">
                                    <input type="radio" name="edit-priority" value="low" class="text-emerald-500 focus:ring-emerald-500">
                                    <span class="ml-2 text-sm text-gray-700">Low</span>
                                </label>
                                <label class="inline-flex items-center">
                                    <input type="radio" name="edit-priority" value="medium" class="text-amber-500 focus:ring-amber-500">
                                    <span class="ml-2 text-sm text-gray-700">Medium</span>
                                </label>
                                <label class="inline-flex items-center">
                                    <input type="radio" name="edit-priority" value="high" class="text-rose-500 focus:ring-rose-500">
                                    <span class="ml-2 text-sm text-gray-700">High</span>
                                </label>
                            </div>
                        </div>
                    </div>
                    <div class="mt-6 flex justify-end space-x-3">
                        <button type="button" id="cancel-edit" class="px-4 py-2 border border-gray-300 rounded-md text-sm font-medium text-gray-700 hover:bg-gray-50">
                            Cancel
                        </button>
                        <button type="submit" class="px-4 py-2 bg-indigo-600 rounded-md text-sm font-medium text-white hover:bg-indigo-700">
                            Save Changes
                        </button>
                    </div>
                </form>
            </div>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            // DOM Elements
            const taskForm = document.getElementById('task-form');
            const taskList = document.getElementById('task-list');
            const emptyState = document.getElementById('empty-state');
            const titleInput = document.getElementById('title');
            const dateInput = document.getElementById('date');
            const descriptionInput = document.getElementById('description');
            const priorityInputs = document.querySelectorAll('input[name="priority"]');
            const submitBtn = document.getElementById('submit-btn');
            const cancelBtn = document.getElementById('cancel-btn');
            const filterButtons = document.querySelectorAll('.filter-btn');
            const taskCount = document.getElementById('task-count');
            const clearCompletedBtn = document.getElementById('clear-completed');
            const darkModeToggle = document.getElementById('dark-mode-toggle');
            const taskModal = document.getElementById('task-modal');
            const modalTitle = document.getElementById('modal-title');
            const editForm = document.getElementById('edit-form');
            const editTitle = document.getElementById('edit-title');
            const editDescription = document.getElementById('edit-description');
            const editDate = document.getElementById('edit-date');
            const editPriorityInputs = document.querySelectorAll('input[name="edit-priority"]');
            const cancelEditBtn = document.getElementById('cancel-edit');

            // State variables
            let tasks = JSON.parse(localStorage.getItem('tasks')) || [];
            let currentFilter = 'all';
            let isEditing = false;
            let currentEditId = null;

            // Initialize the app
            init();

            // Event Listeners
            taskForm.addEventListener('submit', handleAddTask);
            cancelBtn.addEventListener('click', resetForm);
            clearCompletedBtn.addEventListener('click', clearCompletedTasks);
            darkModeToggle.addEventListener('click', toggleDarkMode);
            editForm.addEventListener('submit', handleEditSubmit);
            cancelEditBtn.addEventListener('click', closeModal);

            // Filter buttons
            filterButtons.forEach(btn => {
                btn.addEventListener('click', () => {
                    setActiveFilter(btn.dataset.filter);
                    filterButtons.forEach(b => {
                        if (b === btn) {
                            b.classList.remove('bg-gray-100', 'text-gray-700');
                            b.classList.add('bg-indigo-100', 'text-indigo-700');
                        } else {
                            b.classList.add('bg-gray-100', 'text-gray-700');
                            b.classList.remove('bg-indigo-100', 'text-indigo-700');
                        }
                    });
                });
            });

            // Set today's date as default for date inputs
            const today = new Date().toISOString().split('T')[0];
            dateInput.min = today;
            dateInput.value = '';

            // Functions
            function init() {
                renderTasks();
                updateTaskCount();
                applyDarkModePreference();
            }

            function handleAddTask(e) {
                e.preventDefault();
                
                const title = titleInput.value.trim();
                const description = descriptionInput.value.trim();
                const date = dateInput.value;
                const priority = Array.from(priorityInputs).find(radio => radio.checked).value;
                
                if (!title) return;
                
                const newTask = {
                    id: Date.now().toString(),
                    title,
                    description,
                    date,
                    priority,
                    completed: false,
                    createdAt: new Date().toISOString()
                };
                
                tasks.push(newTask);
                saveTasks();
                renderTasks();
                resetForm();
                updateTaskCount();
            }

            function resetForm() {
                taskForm.reset();
                isEditing = false;
                submitBtn.textContent = 'Add Task';
                cancelBtn.classList.add('hidden');
                Array.from(priorityInputs).find(radio => radio.value === 'low').checked = true;
            }

            function renderTasks() {
                // Filter tasks based on current filter
                let filteredTasks = [...tasks];
                
                switch (currentFilter) {
                    case 'active':
                        filteredTasks = filteredTasks.filter(task => !task.completed);
                        break;
                    case 'completed':
                        filteredTasks = filteredTasks.filter(task => task.completed);
                        break;
                    case 'today':
                        const today = new Date().toISOString().split('T')[0];
                        filteredTasks = filteredTasks.filter(task => task.date === today);
                        break;
                    case 'high':
                        filteredTasks = filteredTasks.filter(task => task.priority === 'high');
                        break;
                }
                
                if (filteredTasks.length === 0) {
                    taskList.innerHTML = '';
                    emptyState.classList.remove('hidden');
                    emptyState.style.display = 'block';
                    return;
                }
                
                emptyState.style.display = 'none';
                
                const tasksHTML = filteredTasks.map(task => {
                    const formattedDate = task.date ? new Date(task.date).toLocaleDateString('en-US', {
                        weekday: 'short',
                        month: 'short',
                        day: 'numeric'
                    }) : 'No due date';
                    
                    const overdueClass = task.date && new Date(task.date) < new Date() && !task.completed ? 'text-rose-500' : 'text-gray-500';
                    
                    return `
                        <div class="task-item bg-white rounded-xl shadow-sm p-4 fade-in ${task.completed ? 'task-completed' : ''} priority-${task.priority}" data-id="${task.id}">
                            <div class="flex items-start space-x-3">
                                <button class="toggle-complete mt-1 flex-shrink-0 h-5 w-5 rounded-full border ${task.completed ? 'bg-emerald-100 border-emerald-200 flex items-center justify-center' : 'border-gray-300'}" aria-label="${task.completed ? 'Mark as incomplete' : 'Mark as complete'}" data-id="${task.id}">
                                    ${task.completed ? '<svg xmlns="http://www.w3.org/2000/svg" class="h-3 w-3 text-emerald-500" viewBox="0 0 20 20" fill="currentColor"><path fill-rule="evenodd" d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z" clip-rule="evenodd" /></svg>' : ''}
                                </button>
                                <div class="flex-grow min-w-0">
                                    <div class="flex items-center justify-between">
                                        <h3 class="task-title text-gray-800 font-medium truncate">${task.title}</h3>
                                        <div class="flex space-x-2">
                                            <span class="priority-badge text-xs px-2 py-1 rounded-full ${
                                                task.priority === 'high' ? 'bg-rose-100 text-rose-800' : 
                                                task.priority === 'medium' ? 'bg-amber-100 text-amber-800' : 
                                                'bg-emerald-100 text-emerald-800'
                                            }">
                                                ${task.priority.charAt(0).toUpperCase() + task.priority.slice(1)}
                                            </span>
                                        </div>
                                    </div>
                                    ${task.description ? `<p class="text-sm text-gray-600 mt-1 line-clamp-2">${task.description}</p>` : ''}
                                    <div class="flex items-center justify-between mt-2">
                                        <span class="text-xs ${overdueClass}">${formattedDate}</span>
                                        <div class="flex items-center space-x-2">
                                            <button class="edit-btn text-sm text-indigo-600 hover:text-indigo-800" data-id="${task.id}">
                                                Edit
                                            </button>
                                            <button class="delete-btn text-sm text-rose-600 hover:text-rose-800" data-id="${task.id}">
                                                Delete
                                            </button>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    `;
                }).join('');
                
                taskList.innerHTML = tasksHTML;
                
                // Add event listeners to dynamic elements
                document.querySelectorAll('.toggle-complete').forEach(btn => {
                    btn.addEventListener('click', toggleTaskComplete);
                });
                
                document.querySelectorAll('.edit-btn').forEach(btn => {
                    btn.addEventListener('click', openEditModal);
                });
                
                document.querySelectorAll('.delete-btn').forEach(btn => {
                    btn.addEventListener('click', deleteTask);
                });
            }

            function toggleTaskComplete(e) {
                const taskId = e.currentTarget.dataset.id;
                const task = tasks.find(t => t.id === taskId);
                
                if (task) {
                    task.completed = !task.completed;
                    saveTasks();
                    renderTasks();
                    updateTaskCount();
                }
            }

            function openEditModal(e) {
                const taskId = e.currentTarget.dataset.id;
                const task = tasks.find(t => t.id === taskId);
                
                if (task) {
                    currentEditId = taskId;
                    editTitle.value = task.title;
                    editDescription.value = task.description || '';
                    editDate.value = task.date || '';
                    Array.from(editPriorityInputs).find(radio => radio.value === task.priority).checked = true;
                    
                    modalTitle.textContent = `Edit "${task.title.substring(0, 20)}${task.title.length > 20 ? '...' : ''}"`;
                    taskModal.classList.remove('hidden');
                }
            }

            function closeModal() {
                taskModal.classList.add('hidden');
                currentEditId = null;
            }

            function handleEditSubmit(e) {
                e.preventDefault();
                
                if (!currentEditId) return;
                
                const task = tasks.find(t => t.id === currentEditId);
                
                if (task) {
                    task.title = editTitle.value.trim();
                    task.description = editDescription.value.trim();
                    task.date = editDate.value;
                    task.priority = Array.from(editPriorityInputs).find(radio => radio.checked).value;
                    
                    saveTasks();
                    renderTasks();
                    closeModal();
                }
            }

            function deleteTask(e) {
                if (confirm('Are you sure you want to delete this task?')) {
                    const taskId = e.currentTarget.dataset.id;
                    tasks = tasks.filter(task => task.id !== taskId);
                    saveTasks();
                    renderTasks();
                    updateTaskCount();
                }
            }

            function clearCompletedTasks() {
                if (confirm('Are you sure you want to clear all completed tasks?')) {
                    tasks = tasks.filter(task => !task.completed);
                    saveTasks();
                    renderTasks();
                    updateTaskCount();
                }
            }

            function setActiveFilter(filter) {
                currentFilter = filter;
                renderTasks();
            }

            function updateTaskCount() {
                const activeCount = tasks.filter(task => !task.completed).length;
                const totalCount = tasks.length;
                
                if (totalCount === 0) {
                    taskCount.textContent = 'No tasks';
                } else if (activeCount === totalCount) {
                    taskCount.textContent = `${totalCount} ${totalCount === 1 ? 'task' : 'tasks'}`;
                } else {
                    taskCount.textContent = `${activeCount} active of ${totalCount} total ${totalCount === 1 ? 'task' : 'tasks'}`;
                }
            }

            function saveTasks() {
                localStorage.setItem('tasks', JSON.stringify(tasks));
            }

            function toggleDarkMode() {
                const html = document.documentElement;
                const isDark = html.classList.toggle('dark');
                localStorage.setItem('darkMode', isDark ? 'enabled' : 'disabled');
                
                const icon = darkModeToggle.querySelector('img');
                icon.src = isDark ? 'https://storage.googleapis.com/workspace-0f70711f-8b4e-4d94-86f1-2a93ccde5887/image/fa214182-9ca7-42e4-8091-6ff8d4f2daa8.png' : 'https://storage.googleapis.com/workspace-0f70711f-8b4e-4d94-86f1-2a93ccde5887/image/7a8ee53b-9f82-4b83-9051-4c1faf2e3edf.png';
                icon.alt = isDark ? 'Sun icon for light mode' : 'Moon icon for dark mode';
            }

            function applyDarkModePreference() {
                const darkModePreference = localStorage.getItem('darkMode');
                
                if (darkModePreference === 'enabled') {
                    document.documentElement.classList.add('dark');
                    const icon = darkModeToggle.querySelector('img');
                    icon.src = 'https://placehold.co/20/white/white?text=☀️';
                    icon.alt = 'Sun icon for light mode';
                } else if (window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches) {
                    document.documentElement.classList.add('dark');
                    const icon = darkModeToggle.querySelector('img');
                    icon.src = 'https://placehold.co/20/white/white?text=☀️';
                    icon.alt = 'Sun icon for light mode';
                }
            }
        });
    </script>
</body>
</html>

