<template>
    <div id="app">
        <div class="container">
            <div class="row">
                <div class="col-sm">
                    <ul class="task-list">
                        <li class="task" v-for="task in tasks" :key="task.id" @click="activateTask(task.id)">{{ task.title }}</li>
                    </ul>
                </div>
                <transition name="slide-fade">
                    <TaskDetail :task="activeTask" />
                </transition>
            </div>
        </div>
    </div>

</template>

<script>

    import axios from 'axios'
    import TaskDetail from "./components/TaskDetail";

    export default {
        name: 'app',
        components: {TaskDetail},
        data() {
            return {
                activeTask: null,
                tasks: null,
                errors: []
            };
        },
        methods: {
            activateTask: function(id) {
                this.activeTask = this.tasks.find(t => {
                    return t.id === id
                })
            },
            getTaskList: function () {
                axios.get("https://jsonplaceholder.typicode.com/todos/")
                    .then(response => {
                        this.tasks = response.data
                    })
                    .catch(e => {
                        this.errors.push(e)
                    })
            }
        },
        mounted() {
            this.getTaskList()
        }
    }
</script>

<style>
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }

    .task-list {
        padding-inline-start: 0;
    }

    .task-list .task {
        text-align: left;
        padding: 7px;
        border: 1px solid #ececec;
        margin-bottom: -1px;
        list-style: none;
    }

    .slide-fade-enter {
        transform: translateX(500px);
        /*opacity: 0;*/
        background-color: red;
    }

    .slide-fade-enter-active {
        transition: all 0.5s ease;
        background-color: red;
    }

    .slide-fade-leave-active {
        transition: all 0.5s ease;
        background-color: darkblue;
    }

    .slide-fade-leave-to {
        transform: translateX(100%);
        /*opacity: 0;*/
        background-color: darkblue;
    }

</style>
