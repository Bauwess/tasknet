<template>
    <div class="task-detail" v-if="activeTask">
        <div class="loading-wrapper" v-if="loading">
            <content-loader
                    :height="500"
                    :width="440"
                    :speed="2"
                    primaryColor="#f3f3f3"
                    secondaryColor="#e8e8e8"
            >
                <circle cx="10" cy="20" r="8" />
                <rect x="25" y="15" rx="5" ry="5" width="220" height="10" />
                <circle cx="10" cy="50" r="8" />
                <rect x="25" y="45" rx="5" ry="5" width="220" height="10" />
                <circle cx="10" cy="80" r="8" />
                <rect x="25" y="75" rx="5" ry="5" width="220" height="10" />
                <circle cx="10" cy="110" r="8" />
                <rect x="25" y="105" rx="5" ry="5" width="220" height="10" />
                <rect x="321.84" y="40.61" rx="0" ry="0" width="61" height="49" />
            </content-loader>
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
    import { ContentLoader } from 'vue-content-loader'

    export default {
        name: 'TaskDetail',
        components: { ContentLoader },
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
