<template>
  <div id="app">
    <div class="container-fluid pl-5 pr-5">
      <div class="row">
        <div class="col-sm task-list-column">
          <transition-group tag="ul" name="slide-fade-list" appear class="task-list">
            <li class="task" :class="{ active : isTaskActive(task.id) }" v-for="task in tasks" :key="task.id">
              <div class="remove-task"><span @click="removeTask(task.id)">[X]</span></div>
              <div class="task-title" @click="activateTask(task.id)">{{ task.title }}</div>
            </li>
          </transition-group>
        </div>
        <transition name="slide-fade" mode="out-in">
          <div class="col-sm-5 task-detail-pane" v-show="paneOpen">
            <span class="pull-right close-pane" @click="closePane">X</span>
            <TaskDetail />
          </div>
        </transition>
      </div>
    </div>
  </div>

</template>

<script>

  import axios from 'axios'
  import { EventBus } from "./main"; // check the path
  import TaskDetail from "./components/TaskDetail.vue";

  export default {
    name: 'app',
    components: {TaskDetail},
    data() {
      return {
        activeTaskId: null,
        paneOpen: false,
        tasks: null,
        errors: []
      };
    },
    methods: {
      openPane: function(){
        this.paneOpen = true;
      },
      closePane: function(){
        this.paneOpen = false;
        this.activeTaskId = null
      },
      activateTask: function(id) {
        this.openPane();
        this.activeTaskId = id;
        EventBus.$emit('TASK_DETAIL', id);
      },
      removeTask: function(id){
        let index = this.tasks.findIndex(task => task.id === id);
        this.tasks.splice(index, 1);
        if(this.activeTaskId === id){
          this.closePane();
        }
      },
      isTaskActive: function(id){
        if(this.activeTaskId !== null && this.activeTaskId !== undefined){
          return this.activeTaskId === id
        }
        return false
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
    margin-top: 50px;
  }

  .task-list-column {
    height: 90vh;
    overflow-y: scroll;
    background-color: #ffffff;
    border: 1px solid #ececec;
    margin-right: 10px;

  }

  .task-detail-pane {
    background-color: #ffffff;
    border: 1px solid #ececec;
  }

  .task-list {
    padding-inline-start: 0;
  }

  .task-list .task .remove-task {
    width: 40px;
    display: inline-block;
  }

  .task-list .task .remove-task:hover {
    color: darkred;
    font-weight: bold;
  }

  .task-list .task .task-title {
    width: calc(100% - 40px);
    height: 100%;
    display: flex;
    align-items: center;
  }
  .task-list .task {
    display: flex;
    align-items: center;
    font-size: 13px;
    text-align: left;
    padding: 0 7px;
    height: 36px;
    border-top: 1px solid #ececec;
    border-bottom: 1px solid #ececec;
    margin-bottom: -1px;
    list-style: none;
    cursor: pointer;
  }

  .task-list .task:first-child {
    border-top: none;
  }

  .task-list .task:last-child {
    border-bottom: none;
  }

  .task-list .task.active {
    background-color: #edf8ff;
  }

  .task-detail-pane .close-pane {
    position: absolute;
    right: 10px;
    top: 10px;
    cursor: pointer;
  }

  .slide-fade-enter {
    transform: translateX(500px);
    opacity: 0;
  }

  .slide-fade-enter-active {
    transition: all 0.5s ease;
  }

  .slide-fade-leave-active {
    transition: all 0.5s ease;
  }

  .slide-fade-leave-to {
    transform: translateX(100%);
    opacity: 0;
  }

  .slide-fade-list-enter {
    transform: translateX(30px);
    opacity: 0;
  }

  .slide-fade-list-enter-active {
    transition: all 0.5s ease;
  }

  .slide-fade-list-leave-active {
    transition: all 0.5s ease;
    background-color: darkred!important;
  }

  .slide-fade-list-leave-to {
    transform: translateX(-100%);
    opacity: 0;
  }

</style>
