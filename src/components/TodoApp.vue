<script>

export default {
    data() {
        return {
            newTodo: "",
            todos: [],
        };
    },

    mounted() {
        const todosData = JSON.parse(localStorage.getItem("todos"));
        if (todosData) {
            this.todos = todosData;
        }
    },

    methods: {
        addTodo() {
            if (this.newTodo) {
                this.todos.push({
                    done: false,
                    content: this.newTodo,
                });
                this.newTodo = "";
            }
            this.saveData();
        },

        doneTodo: function (todo) {
            todo.done = !todo.done;
            this.saveData();
        },

        removeTodo: function (index) {
            this.todos.splice(index, 1);
            this.saveData();
        },

        saveData() {
            const storageData = JSON.stringify(this.todos);
            localStorage.setItem("todos", storageData);
        },
    },
};
</script>
<template>
    <div class="container">
        <div class="row">
            <div class="upper col-12">
                <img src="./../assets/vue.svg" class="logo vue" alt="Vue logo" />
                <h1>Todo List App</h1>
                <h3>{{ newTodo }}</h3>
            </div>
            <div class="col-6 ">
                <form @submit.prevent="addTodo()">
                    <input type="text" v-model="newTodo" name="newTodo" class="form-control mb-2" placeholder="Name Todos">
                    <button class="btn-primary">Add Todo</button>
                </form>
            </div>
            <div class="col-6 ">
                <table class="col-12">
                    <ul>
                        <li v-for="(todo, index) in todos" :key="index">
                                <tr>
                                    <td><span :class="{ done: todo.done }" @click="doneTodo(todo)">{{ todo.content }}</span></td>
                                    <td><button class="btn-remove" @click="removeTodo(index)"><i class="fa fa-trash"></i></button></td>
                                </tr>
                        </li>
                        <div class="d-grid gap-2">
                            <button class=" btn-block empty" v-if="todos.length === 0">List Empty</button>
                        </div>
                    </ul>
                </table>
            </div>
        </div>
    </div>
</template>