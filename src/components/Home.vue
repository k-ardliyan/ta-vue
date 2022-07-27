<template>
    <div class="container">
        <div class="row pt-5">
            <div class="col-md-12">
                <div class="card">
                    <div class="card-body">
                        <div class="row">
                            <div class="col align-self-center">
                                <div class="form-floating">
                                    <input type="text" class="form-control" id="inputTodo" v-model="inputTodo" placeholder="Tambah Todo"
                                        @keyup.enter="addTodo()">
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
                <div class="card">
                    <div class="card-body">
                        <ul class="nav nav-pills mb-3" id="pills-tab" role="tablist">
                            <li class="nav-item" role="presentation">
                                <button class="nav-link active" id="pills-todoActive-tab" data-bs-toggle="pill"
                                    data-bs-target="#pills-todoActive" type="button" role="tab"
                                    aria-controls="pills-todoActive" aria-selected="true">Todo</button>
                            </li>
                            <li class="nav-item" role="presentation">
                                <button class="nav-link" id="pills-todoDone-tab" data-bs-toggle="pill"
                                    data-bs-target="#pills-todoDone" type="button" role="tab"
                                    aria-controls="pills-todoDone" aria-selected="false">Complete</button>
                            </li>
                        </ul>
                        <!-- Todo Undone -->
                        <div class="tab-content" id="pills-tabContent">
                            <div class="tab-pane fade show active" id="pills-todoActive" role="tabpanel"
                                aria-labelledby="pills-todoActive-tab" tabindex="0">
                                    <transition-group name="list" tag="ul" class="list-group">
                                        <li class="list-group-item" v-if="todoActive.length < 1">
                                            Tidak ada todo
                                        </li>
                                        <li v-else class="list-group-item" v-for="todo in todoActive" :key="todo">
                                            <input class="form-check-input me-1" type="checkbox" :value="todo.name"
                                                :id="todo.id" @click="checkTodo(todo.id)" :checked="todo.isDone">
                                            <label class="form-check-label stretched-link"
                                                :for="todo.id">{{ todo.name }}</label>
                                        </li>
                                    </transition-group>
                            </div>
                            <!-- Todo Done -->
                            <div class="tab-pane fade" id="pills-todoDone" role="tabpanel"
                                aria-labelledby="pills-todoDone-tab" tabindex="0">
                                <transition-group name="list" tag="ul" class="list-group">
                                    <li class="list-group-item" v-if="todoDone.length < 1">
                                        Tidak ada todo yang diselesaikan
                                    </li>
                                        <li v-else class="list-group-item" v-for="todo in todoDone" :key="todo">
                                            <input class="form-check-input me-1" type="checkbox" :value="todo.name"
                                                :id="todo.id" @click="uncheckTodo(todo.id)" :checked="todo.isDone">
                                            <label class="form-check-label stretched-link"
                                                :for="todo.id">{{ todo.name }}</label>
                                        </li>
                                </transition-group>
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
            return {
                todoActive: [],
                todoDone: []
            }
        },
        computed: {
            // checklist() {

            // }
        },
        methods: {
            addTodo() {
                this.todoActive.push({
                    id: +new Date(),
                    name: this.inputTodo,
                    isDone: false
                })
                this.inputTodo = ''
            },
            checkTodo(id) {
                const todoCheck = this.todoActive.find(todo => todo.id === id)
                todoCheck.isDone = true
                if (todoCheck.isDone) {
                    this.todoDone.push(todoCheck)
                    this.todoActive = this.todoActive.filter(todo => todo.id !== id)
                } else {
                    this.todoActive.push(todoCheck)
                    this.todoDone = this.todoDone.filter(todo => todo.id !== id)
                }
            },
            uncheckTodo(id) {
                const todoUncheck = this.todoDone.find(todo => todo.id === id)
                todoUncheck.isDone = false
                if (!todoUncheck.isDone) {
                    this.todoActive.push(todoUncheck)
                    this.todoDone = this.todoDone.filter(todo => todo.id !== id)
                } else {
                    this.todoDone.push(todoUncheck)
                    this.todoActive = this.todoActive.filter(todo => todo.id !== id)
                }
            }
        },
    }
</script>

<style scoped>
.list-move, /* apply transition to moving elements */
.list-enter-active,
.list-leave-active {
  transition: all 0.5s ease;
}

.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translateX(30px);
}
</style>
