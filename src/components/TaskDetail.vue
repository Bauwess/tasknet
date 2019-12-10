<template>
    <div class="task-detail" v-if="activeTask">
        <div class="loading-wrapper" v-if="loading">
            <img src="https://miro.medium.com/max/882/1*9EBHIOzhE1XfMYoKz1JcsQ.gif" />
        </div>
        <transition name="slide-fade-detail" mode="in-out">
            <div v-if="!loading">
                <h2>{{ activeTask.title }}</h2>
            </div>
        </transition>
    </div>
</template>

<script>

    import axios from 'axios'
    import { EventBus } from "../main"; // check the path

    export default {
        name: 'TaskDetail',
        data() {
            return {
                activeTask: null,
                loading: false
            };
        },
        methods: {
            getTask: function (id) {
                this.loading = true;
                axios.get("https://jsonplaceholder.typicode.com/todos/" + id)
                    .then(response => {
                        this.activeTask = response.data;
                        this.loading = false;
                    })
                    .catch(e => {
                        this.errors.push(e)
                    })
            }
        },
        created() {
            EventBus.$on('TASK_DETAIL', id => {
                this.getTask(id);
            });
        }
    }

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
    h3 {
        margin: 40px 0 0;
    }

    ul {
        list-style-type: none;
        padding: 0;
    }

    li {
        display: inline-block;
        margin: 0 10px;
    }

    a {
        color: #42b983;
    }

    .slide-fade-detail-enter {
        transform: translateY(10px);
        opacity: 0;
    }

    .slide-fade-detail-enter-active {
        transition: all 0.5s ease;
    }

    .task-detail .loading-wrapper {
        position: absolute;
        width: 98%;
    }

    /*.slide-fade-detail-leave-active {*/
    /*    transition: all 0.5s ease;*/
    /*}*/

    /*.slide-fade-detail-leave-to {*/
    /*    transform: translateY(50px);*/
    /*    opacity: 0;*/
    /*}*/
</style>
