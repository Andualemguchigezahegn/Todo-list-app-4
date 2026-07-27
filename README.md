<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Todo List App</title>

    <!-- Font Awesome for Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css" />

    <style>
        /* ========== CSS VARIABLES ========== */
        :root {
            --bg-primary: #f0f2f5;
            --bg-secondary: #ffffff;
            --text-primary: #1a1a2e;
            --text-secondary: #555;
            --accent: #6c63ff;
            --accent-hover: #5a52d5;
            --success: #28a745;
            --danger: #dc3545;
            --warning: #ffc107;
            --shadow: 0 10px 30px rgba(0, 0, 0, 0.08);
            --border-radius: 16px;
            --transition: 0.3s ease;
        }

        [data-theme="dark"] {
            --bg-primary: #0f0f1a;
            --bg-secondary: #1a1a2e;
            --text-primary: #f0f0f5;
            --text-secondary: #b0b0c0;
            --shadow: 0 10px 30px rgba(0, 0, 0, 0.4);
        }

        /* ========== RESET & BASE ========== */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: var(--bg-primary);
            color: var(--text-primary);
            transition: background 0.4s, color 0.4s;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        /* ========== APP CONTAINER ========== */
        .app-container {
            max-width: 600px;
            width: 100%;
            background: var(--bg-secondary);
            border-radius: var(--border-radius);
            box-shadow: var(--shadow);
            padding: 40px 32px;
            transition: background 0.4s, box-shadow 0.4s;
        }

        /* ========== HEADER ========== */
        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 32px;
        }

        .header h1 {
            font-size: 2rem;
            font-weight: 700;
            color: var(--text-primary);
        }

        .header h1 span {
            color: var(--accent);
        }

        .header-controls {
            display: flex;
            gap: 12px;
            align-items: center;
        }

        #theme-toggle {
            background: none;
            border: none;
            font-size: 1.3rem;
            cursor: pointer;
            color: var(--text-primary);
            transition: transform 0.3s;
            padding: 4px 8px;
        }

        #theme-toggle:hover {
            transform: rotate(20deg);
        }

        /* ========== STATS ========== */
        .stats {
            display: flex;
            justify-content: space-between;
            background: var(--bg-primary);
            padding: 14px 20px;
            border-radius: 12px;
            margin-bottom: 24px;
            font-size: 0.9rem;
        }

        .stats span {
            font-weight: 600;
        }

        .stats .count {
            color: var(--accent);
        }

        /* ========== INPUT SECTION ========== */
        .input-section {
            display: flex;
            gap: 12px;
            margin-bottom: 24px;
        }

        .input-section input {
            flex: 1;
            padding: 14px 18px;
            border: 2px solid var(--bg-primary);
            border-radius: 12px;
            background: var(--bg-primary);
            color: var(--text-primary);
            font-size: 1rem;
            transition: border 0.3s;
            font-family: inherit;
        }

        .input-section input:focus {
            outline: none;
            border-color: var(--accent);
        }

        .input-section input::placeholder {
            color: var(--text-secondary);
        }

        .input-section button {
            padding: 14px 24px;
            background: var(--accent);
            color: #fff;
            border: none;
            border-radius: 12px;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            transition: background 0.3s, transform 0.2s;
            white-space: nowrap;
        }

        .input-section button:hover {
            background: var(--accent-hover);
            transform: scale(1.02);
        }

        /* ========== FILTER BUTTONS ========== */
        .filters {
            display: flex;
            gap: 8px;
            margin-bottom: 24px;
            flex-wrap: wrap;
        }

        .filter-btn {
            padding: 8px 20px;
            border: 2px solid var(--bg-primary);
            background: var(--bg-primary);
            color: var(--text-secondary);
            border-radius: 50px;
            cursor: pointer;
            font-weight: 600;
            transition: 0.3s;
            font-size: 0.85rem;
        }

        .filter-btn.active,
        .filter-btn:hover {
            background: var(--accent);
            color: #fff;
            border-color: var(--accent);
        }

        /* ========== TODO LIST ========== */
        .todo-list {
            list-style: none;
            margin-bottom: 20px;
            max-height: 400px;
            overflow-y: auto;
        }

        .todo-list::-webkit-scrollbar {
            width: 6px;
        }

        .todo-list::-webkit-scrollbar-track {
            background: var(--bg-primary);
            border-radius: 10px;
        }

        .todo-list::-webkit-scrollbar-thumb {
            background: var(--accent);
            border-radius: 10px;
        }

        .todo-item {
            display: flex;
            align-items: center;
            gap: 14px;
            padding: 14px 16px;
            background: var(--bg-primary);
            border-radius: 12px;
            margin-bottom: 10px;
            transition: all 0.3s ease;
            animation: slideIn 0.3s ease;
        }

        .todo-item:hover {
            transform: translateX(4px);
            box-shadow: var(--shadow);
        }

        @keyframes slideIn {
            from {
                opacity: 0;
                transform: translateY(-10px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .todo-item.completed {
            opacity: 0.6;
        }

        .todo-item.completed .todo-text {
            text-decoration: line-through;
            color: var(--text-secondary);
        }

        .todo-item .checkbox {
            width: 22px;
            height: 22px;
            min-width: 22px;
            border: 2px solid var(--accent);
            border-radius: 50%;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.3s;
            background: transparent;
        }

        .todo-item .checkbox:hover {
            background: var(--accent);
            opacity: 0.3;
        }

        .todo-item .checkbox.checked {
            background: var(--accent);
            border-color: var(--accent);
        }

        .todo-item .checkbox.checked i {
            display: block;
            color: #fff;
            font-size: 12px;
        }

        .todo-item .checkbox i {
            display: none;
        }

        .todo-item .todo-text {
            flex: 1;
            font-size: 1rem;
            color: var(--text-primary);
            word-break: break-word;
        }

        .todo-item .todo-actions {
            display: flex;
            gap: 8px;
        }

        .todo-item .todo-actions button {
            background: none;
            border: none;
            cursor: pointer;
            padding: 4px 8px;
            border-radius: 8px;
            transition: all 0.3s;
            font-size: 1rem;
            color: var(--text-secondary);
        }

        .todo-item .todo-actions .edit-btn:hover {
            background: var(--warning);
            color: #fff;
        }

        .todo-item .todo-actions .delete-btn:hover {
            background: var(--danger);
            color: #fff;
        }

        /* ========== EDIT MODE ========== */
        .todo-item.editing .todo-text {
            display: none;
        }

        .todo-item.editing .edit-input {
            display: block;
        }

        .edit-input {
            display: none;
            flex: 1;
            padding: 8px 12px;
            border: 2px solid var(--accent);
            border-radius: 8px;
            background: var(--bg-secondary);
            color: var(--text-primary);
            font-size: 1rem;
            font-family: inherit;
        }

        .edit-input:focus {
            outline: none;
        }

        /* ========== CLEAR BUTTON ========== */
        .clear-btn {
            width: 100%;
            padding: 12px;
            background: var(--danger);
            color: #fff;
            border: none;
            border-radius: 12px;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            transition: opacity 0.3s, transform 0.2s;
            opacity: 0.7;
        }

        .clear-btn:hover {
            opacity: 1;
            transform: scale(1.01);
        }

        .clear-btn:disabled {
            opacity: 0.3;
            cursor: not-allowed;
            transform: none;
        }

        /* ========== EMPTY STATE ========== */
        .empty-state {
            text-align: center;
            padding: 40px 20px;
            color: var(--text-secondary);
        }

        .empty-state i {
            font-size: 3rem;
            margin-bottom: 16px;
            opacity: 0.3;
        }

        .empty-state p {
            font-size: 1.1rem;
        }

        /* ========== RESPONSIVE ========== */
        @media (max-width: 480px) {
            .app-container {
                padding: 24px 16px;
            }

            .header h1 {
                font-size: 1.5rem;
            }

            .input-section {
                flex-direction: column;
            }

            .input-section button {
                width: 100%;
            }

            .stats {
                flex-direction: column;
                gap: 6px;
                text-align: center;
            }

            .filters {
                justify-content: center;
            }

            .todo-item {
                flex-wrap: wrap;
                gap: 10px;
            }

            .todo-item .todo-text {
                width: 100%;
                order: 3;
            }

            .todo-item .todo-actions {
                margin-left: auto;
            }
        }
    </style>
</head>
<body>

    <div class="app-container">
        <!-- ========== HEADER ========== -->
        <div class="header">
            <h1>📋 <span>Todo</span> List</h1>
            <div class="header-controls">
                <button id="theme-toggle" aria-label="Toggle theme">
                    <i class="fas fa-moon"></i>
                </button>
            </div>
        </div>

        <!-- ========== STATS ========== -->
        <div class="stats">
            <span>Total: <span class="count" id="total-count">0</span></span>
            <span>Completed: <span class="count" id="completed-count">0</span></span>
            <span>Pending: <span class="count" id="pending-count">0</span></span>
        </div>

        <!-- ========== INPUT SECTION ========== -->
        <div class="input-section">
            <input type="text" id="todo-input" placeholder="Add a new task..." maxlength="100" />
            <button id="add-btn"><i class="fas fa-plus"></i> Add</button>
        </div>

        <!-- ========== FILTERS ========== -->
        <div class="filters">
            <button class="filter-btn active" data-filter="all">All</button>
            <button class="filter-btn" data-filter="active">Active</button>
            <button class="filter-btn" data-filter="completed">Completed</button>
        </div>

        <!-- ========== TODO LIST ========== -->
        <ul class="todo-list" id="todo-list">
            <!-- Todos will be rendered here -->
        </ul>

        <!-- ========== CLEAR ALL ========== -->
        <button class="clear-btn" id="clear-btn" disabled>
            <i class="fas fa-trash"></i> Clear All Completed
        </button>
    </div>

    <!-- ========== JAVASCRIPT ========== -->
    <script>
        document.addEventListener('DOMContentLoaded', function() {

            // =============================================
            // STATE
            // =============================================
            let todos = [];
            let currentFilter = 'all';
            let editingId = null;

            // =============================================
            // DOM REFERENCES
            // =============================================
            const todoInput = document.getElementById('todo-input');
            const addBtn = document.getElementById('add-btn');
            const todoList = document.getElementById('todo-list');
            const totalCount = document.getElementById('total-count');
            const completedCount = document.getElementById('completed-count');
            const pendingCount = document.getElementById('pending-count');
            const clearBtn = document.getElementById('clear-btn');
            const filterBtns = document.querySelectorAll('.filter-btn');
            const themeToggle = document.getElementById('theme-toggle');
            const themeIcon = themeToggle.querySelector('i');

            // =============================================
            // LOAD FROM LOCAL STORAGE
            // =============================================
            function loadTodos() {
                const stored = localStorage.getItem('todos');
                if (stored) {
                    todos = JSON.parse(stored);
                } else {
                    // Sample todos
                    todos = [
                        { id: Date.now() + 1, text: 'Learn JavaScript', completed: false },
                        { id: Date.now() + 2, text: 'Build a Todo App', completed: true },
                        { id: Date.now() + 3, text: 'Deploy to GitHub', completed: false },
                    ];
                }
                render();
            }

            function saveTodos() {
                localStorage.setItem('todos', JSON.stringify(todos));
            }

            // =============================================
            // RENDER
            // =============================================
            function render() {
                // Filter todos
                let filteredTodos = todos;
                if (currentFilter === 'active') {
                    filteredTodos = todos.filter(t => !t.completed);
                } else if (currentFilter === 'completed') {
                    filteredTodos = todos.filter(t => t.completed);
                }

                // Update stats
                const total = todos.length;
                const completed = todos.filter(t => t.completed).length;
                const pending = total - completed;

                totalCount.textContent = total;
                completedCount.textContent = completed;
                pendingCount.textContent = pending;

                // Enable/disable clear button
                clearBtn.disabled = completed === 0;

                // Render list
                if (filteredTodos.length === 0) {
                    todoList.innerHTML = `
                                <div class="empty-state">
                                    <i class="fas fa-clipboard-list"></i>
                                    <p>${currentFilter === 'all' ? 'No tasks yet. Add one above!' : 
                                      currentFilter === 'active' ? 'All tasks completed! 🎉' : 
                                      'No completed tasks yet.'}</p>
                                </div>
                            `;
                    return;
                }

                todoList.innerHTML = filteredTodos.map(todo => `
                            <li class="todo-item ${todo.completed ? 'completed' : ''} ${editingId === todo.id ? 'editing' : ''}" data-id="${todo.id}">
                                <div class="checkbox ${todo.completed ? 'checked' : ''}" data-action="toggle">
                                    <i class="fas fa-check"></i>
                                </div>
                                <span class="todo-text">${escapeHtml(todo.text)}</span>
                                <input type="text" class="edit-input" value="${escapeHtml(todo.text)}" maxlength="100" />
                                <div class="todo-actions">
                                    <button class="edit-btn" data-action="edit" title="Edit">
                                        <i class="fas fa-pen"></i>
                                    </button>
                                    <button class="delete-btn" data-action="delete" title="Delete">
                                        <i class="fas fa-trash"></i>
                                    </button>
                                </div>
                            </li>
                        `).join('');

                // Save to localStorage
                saveTodos();
            }

            // =============================================
            // ESCAPE HTML (for security)
            // =============================================
            function escapeHtml(text) {
                const div = document.createElement('div');
                div.textContent = text;
                return div.innerHTML;
            }

            // =============================================
            // ADD TODO
            // =============================================
            function addTodo() {
                const text = todoInput.value.trim();
                if (!text) {
                    todoInput.focus();
                    todoInput.style.borderColor = '#dc3545';
                    setTimeout(() => {
                        todoInput.style.borderColor = '';
                    }, 1500);
                    return;
                }

                const newTodo = {
                    id: Date.now(),
                    text: text,
                    completed: false,
                };

                todos.push(newTodo);
                todoInput.value = '';
                todoInput.focus();
                render();
            }

            // =============================================
            // TOGGLE TODO
            // =============================================
            function toggleTodo(id) {
                const todo = todos.find(t => t.id === id);
                if (todo) {
                    todo.completed = !todo.completed;
                    render();
                }
            }

            // =============================================
            // DELETE TODO
            // =============================================
            function deleteTodo(id) {
                if (confirm('Delete this task?')) {
                    todos = todos.filter(t => t.id !== id);
                    if (editingId === id) editingId = null;
                    render();
                }
            }

            // =============================================
            // START EDIT
            // =============================================
            function startEdit(id) {
                if (editingId === id) {
                    // If already editing, save changes
                    saveEdit(id);
                    return;
                }

                const todo = todos.find(t => t.id === id);
                if (todo) {
                    editingId = id;
                    render();
                    // Focus the edit input after render
                    setTimeout(() => {
                        const editInput = document.querySelector(`.todo-item[data-id="${id}"] .edit-input`);
                        if (editInput) {
                            editInput.focus();
                            editInput.select();
                        }
                    }, 50);
                }
            }

            // =============================================
            // SAVE EDIT
            // =============================================
            function saveEdit(id) {
                const todo = todos.find(t => t.id === id);
                if (!todo) return;

                const editInput = document.querySelector(`.todo-item[data-id="${id}"] .edit-input`);
                if (!editInput) return;

                const newText = editInput.value.trim();
                if (!newText) {
                    // If empty, cancel edit
                    editingId = null;
                    render();
                    return;
                }

                todo.text = newText;
                editingId = null;
                render();
            }

            // =============================================
            // CLEAR COMPLETED
            // =============================================
            function clearCompleted() {
                const completed = todos.filter(t => t.completed);
                if (completed.length === 0) return;

                if (confirm(`Delete ${completed.length} completed task(s)?`)) {
                    todos = todos.filter(t => !t.completed);
                    render();
                }
            }

            // =============================================
            // EVENT DELEGATION (for todo items)
            // =============================================
            todoList.addEventListener('click', function(e) {
                const target = e.target.closest('[data-action]');
                if (!target) return;

                const li = target.closest('.todo-item');
                if (!li) return;

                const id = parseInt(li.dataset.id);
                const action = target.dataset.action;

                if (action === 'toggle') {
                    toggleTodo(id);
                } else if (action === 'delete') {
                    deleteTodo(id);
                } else if (action === 'edit') {
                    startEdit(id);
                }
            });

            // =============================================
            // KEYBOARD EVENTS (for edit inputs)
            // =============================================
            todoList.addEventListener('keydown', function(e) {
                if (e.key === 'Enter') {
                    const target = e.target;
                    if (target.classList.contains('edit-input')) {
                        const li = target.closest('.todo-item');
                        if (li) {
                            const id = parseInt(li.dataset.id);
                            saveEdit(id);
                        }
                    }
                }

                if (e.key === 'Escape') {
                    const target = e.target;
                    if (target.classList.contains('edit-input')) {
                        editingId = null;
                        render();
                    }
                }
            });

            // =============================================
            // ADD BUTTON
            // =============================================
            addBtn.addEventListener('click', addTodo);

            todoInput.addEventListener('keydown', function(e) {
                if (e.key === 'Enter') {
                    addTodo();
                }
            });

            // =============================================
            // FILTER BUTTONS
            // =============================================
            filterBtns.forEach(btn => {
                btn.addEventListener('click', function() {
                    filterBtns.forEach(b => b.classList.remove('active'));
                    this.classList.add('active');
                    currentFilter = this.dataset.filter;
                    render();
                });
            });

            // =============================================
            // CLEAR BUTTON
            // =============================================
            clearBtn.addEventListener('click', clearCompleted);

            // =============================================
            // THEME TOGGLE
            // =============================================
            let currentTheme = localStorage.getItem('todo-theme') || 'light';
            document.documentElement.setAttribute('data-theme', currentTheme);
            updateIcon(currentTheme);

            themeToggle.addEventListener('click', function() {
                let theme = document.documentElement.getAttribute('data-theme');
                let newTheme = theme === 'light' ? 'dark' : 'light';
                document.documentElement.setAttribute('data-theme', newTheme);
                localStorage.setItem('todo-theme', newTheme);
                updateIcon(newTheme);
            });

            function updateIcon(theme) {
                if (theme === 'dark') {
                    themeIcon.className = 'fas fa-sun';
                } else {
                    themeIcon.className = 'fas fa-moon';
                }
            }

            // =============================================
            // INIT
            // =============================================
            loadTodos();

        });
    </script>

</body>
</html>
