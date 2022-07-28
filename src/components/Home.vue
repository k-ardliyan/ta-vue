<template>
    <div class="min-vh-100" :class="darkMode ? 'bg-dark' : ''">
        <div class="container">
        <div class="row pt-5">
            <div class="col-md-12">
                <div class="form-check form-switch d-flex justify-content-center">
                    <input class="form-check-input" type="checkbox" role="switch" id="darkMode" v-model="darkMode" @change="darkModeToggle()" :checked="darkMode">
                    <label class="form-check-label ms-3" for="darkMode" :class="darkMode ? 'text-white' : ''">
                        <span v-if="darkMode">🌙</span>
                        <span v-else>☀️</span>
                        Mode
                    </label>
                </div>
                <h3 class="display-3 text-center mb-5" :class="darkMode ? 'text-white' : ''">Todo List</h3>
                <div class="card" :class="darkMode ? 'bg-transparent border-0' : ''">
                    <div class="card-body rounded-3" :class="darkMode ? 'bg-dark-opacity' : ''">
                        <div class="row">
                            <div class="col align-self-center">
                                <div class="form-floating">
                                    <input type="text" class="form-control" id="inputTodo" v-model="inputTodo"
                                        placeholder="Tambah Todo" @keyup.enter="addTodo()">
                                    <label for="inputTodo">Tambah Todo</label>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="row pt-3">
            <div class="col-md-12">
                <div class="card" :class="darkMode ? 'bg-transparent border-0' : ''">
                    <div class="card-body rounded-3" :class="darkMode ? 'bg-dark-opacity' : ''">
                        <ul class="nav nav-pills mb-3" id="pills-tab" role="tablist">
                            <!-- Button Menu Todo Active -->
                            <li class="nav-item" role="presentation">
                                <button class="nav-link active position-relative me-4" id="pills-todoActive-tab"
                                    data-bs-toggle="pill" data-bs-target="#pills-todoActive" type="button" role="tab"
                                    aria-controls="pills-todoActive" aria-selected="true">
                                    Todo
                                    <span
                                        class="position-absolute top-0 start-100 translate-middle badge rounded-pill bg-danger"
                                        v-if="todoUndoneLength > 0">
                                        {{ todoUndoneLength }}
                                        <span class="visually-hidden">unread todo active</span>
                                    </span>
                                </button>
                            </li>
                            <!-- Button Menu Todo Complete -->
                            <li class="nav-item" role="presentation">
                                <button class="nav-link position-relative" id="pills-todoDone-tab" data-bs-toggle="pill"
                                    data-bs-target="#pills-todoDone" type="button" role="tab"
                                    aria-controls="pills-todoDone" aria-selected="false">
                                    Complete
                                    <span
                                        class="position-absolute top-0 start-100 translate-middle badge rounded-pill bg-info"
                                        v-if="todoDoneLength > 0">
                                        {{ todoDoneLength }}
                                        <span class="visually-hidden">unread todo done</span>
                                    </span>
                                </button>
                            </li>
                        </ul>

                        <!-- Todo Active -->
                        <div class="tab-content" id="pills-tabContent">
                            <div class="tab-pane fade show active" id="pills-todoActive" role="tabpanel"
                                aria-labelledby="pills-todoActive-tab" tabindex="0">
                                <transition-group name="list" tag="ul" class="list-group">
                                    <li class="list-unstyled" v-if="!todoUndoneLength > 0" :class="darkMode ? 'text-white' : ''">
                                        Tidak ada todo
                                    </li>
                                    <li v-else class="list-group-item d-flex align-items-center" v-for="todo in todoUndone" :key="todo"  :class="darkMode ? 'bg-dark-3 text-white' : ''">
                                        <input class="form-check-input me-1"  type="checkbox" :value="todo.name" :id="todo.id" @change="checkTodo(todo.id)" :checked="todo.isDone">
                                        <label class="form-check-label" :for="todo.id">{{ todo.name }}</label>
                                        <div class="ms-auto">
                                            <button class="btn btn-sm" @click="editTodo(todo.id)">
                                                <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" style="fill: rgba(245, 213, 39, 0.8);transform: ;msFilter:;"><path d="m7 17.013 4.413-.015 9.632-9.54c.378-.378.586-.88.586-1.414s-.208-1.036-.586-1.414l-1.586-1.586c-.756-.756-2.075-.752-2.825-.003L7 12.583v4.43zM18.045 4.458l1.589 1.583-1.597 1.582-1.586-1.585 1.594-1.58zM9 13.417l6.03-5.973 1.586 1.586-6.029 5.971L9 15.006v-1.589z"></path><path d="M5 21h14c1.103 0 2-.897 2-2v-8.668l-2 2V19H8.158c-.026 0-.053.01-.079.01-.033 0-.066-.009-.1-.01H5V5h6.847l2-2H5c-1.103 0-2 .897-2 2v14c0 1.103.897 2 2 2z"></path></svg>
                                            </button>
                                            <button class="btn btn-sm" @click="deleteTodo(todo.id)">
                                                <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24"
                                                    style="fill: rgba(245, 0, 0, 0.8);">
                                                    <path d="M5 20a2 2 0 0 0 2 2h10a2 2 0 0 0 2-2V8h2V6h-4V4a2 2 0 0 0-2-2H9a2 2 0 0 0-2 2v2H3v2h2zM9 4h6v2H9zM8 8h9v12H7V8z"></path><path d="M9 10h2v8H9zm4 0h2v8h-2z"></path>
                                                </svg>
                                            </button>
                                        </div>
                                    </li>
                                </transition-group>
                            </div>

                            <!-- Todo Done -->
                            <div class="tab-pane fade" id="pills-todoDone" role="tabpanel"
                                aria-labelledby="pills-todoDone-tab" tabindex="0">
                                <transition-group name="list" tag="ul" class="list-group">
                                    <li class="list-unstyled" v-if="!todoDoneLength > 0" :class="darkMode ? 'text-white' : ''">
                                        Tidak ada todo yang diselesaikan
                                    </li>
                                    <li v-else class="list-group-item d-flex align-items-center" v-for="todo in todoDone" :key="todo" :class="darkMode ? 'bg-dark-3 text-white' : ''">
                                        <input class="form-check-input me-1" type="checkbox" :value="todo.name"
                                            :id="todo.id" @change="checkTodo(todo.id)" :checked="todo.isDone">
                                        <label class="form-check-label"
                                            :for="todo.id">{{ todo.name }}</label>
                                        <div class="ms-auto">
                                            <button class="btn btn-sm ms-auto" @click="deleteTodo(todo.id)">
                                                <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" style="fill: rgba(245, 0, 0, 0.8);">
                                                    <path d="M5 20a2 2 0 0 0 2 2h10a2 2 0 0 0 2-2V8h2V6h-4V4a2 2 0 0 0-2-2H9a2 2 0 0 0-2 2v2H3v2h2zM9 4h6v2H9zM8 8h9v12H7V8z"></path>
                                                    <path d="M9 10h2v8H9zm4 0h2v8h-2z"></path>
                                                </svg>
                                            </button>
                                        </div>
                                    </li>
                                </transition-group>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'Home',
        data() {
            // get data from localStorage
            let todos = JSON.parse(localStorage.getItem('todos')) || [];
            let darkMode = JSON.parse(localStorage.getItem('darkMode')) || false;
            return {
                todos,
                inputTodo: '',
                darkMode: darkMode,
            }
        },
        computed: {
            // lenght of todos
            todoLength() {
                return this.todos.length;
            },
            // length of todos done
            todoDoneLength() {
                return this.todos.filter(todo => todo.isDone).length;
            },
            // length of todos undone
            todoUndoneLength() {
                return this.todos.filter(todo => !todo.isDone).length;
            },
            // todos undone
            todoUndone() {
                return this.todos.filter(todo => !todo.isDone);
            },
            // todos done
            todoDone() {
                return this.todos.filter(todo => todo.isDone);
            },
        },
        methods: {
            // add todo to local storage
            addTodo() {
                if (this.inputTodo.length > 0) {
                    this.todos.push({
                        name: this.inputTodo,
                        isDone: false,
                        id: +new Date(),
                    });
                    this.inputTodo = '';
                    // add toastr notification
                    toastr.success('Todo berhasil ditambahkan');
                    localStorage.setItem('todos', JSON.stringify(this.todos));
                }
            },
            // delete todo from local storage
            deleteTodo(id) {
                this.todos = this.todos.filter(todo => todo.id !== id)
                localStorage.setItem('todos', JSON.stringify(this.todos))
            },
            // edit todo from local storage
            editTodo(id) {
                let todos = this.todos.find(todo => todo.id === id)
                this.inputTodo = todos.name
                this.deleteTodo(id)
                // focus on input
                document.getElementById('inputTodo').focus()
            },
            // check and uncheck todos from local storage
            checkTodo(id) {
                let todos = this.todos.find(todo => todo.id === id)
                todos.isDone = !todos.isDone
                localStorage.setItem('todos', JSON.stringify(this.todos))
            },
            // clear all todos from local storage
            clearTodo() {
                this.todos = []
                localStorage.setItem('todos', JSON.stringify(this.todos))
            },
            // dark mode add to local storage
            darkModeToggle() {
                if (localStorage.getItem('darkMode') === 'true') {
                    localStorage.setItem('darkMode', 'false');
                } else {
                    localStorage.setItem('darkMode', 'true');
                }
            },
        },
    }
</script>

<style scoped>
    .list-enter-active,
    .list-leave-active {
        transition: opacity 0.3s ease;
    }

    .list-enter-from,
    .list-leave-to {
        opacity: 0;
    }
    .bg-dark {
        background-color: #041C32 !important;
    }
    .bg-dark-opacity {
        background-color: #064663;
    }
    .bg-dark-3 {
        background-color: #04293A;
    }
</style>
