<template>
  <div class="col-sm" v-if="task">
    <span class="pull-right" @click="closeTask">X</span>
    {{ task.id }} {{ task.title }}
  </div>
</template>

<script>
export default {
  name: 'TaskDetail',
  props: ['task'],
  data() {
    return {
      activeTask: this.task
    };
  },
  methods: {
    closeTask: function() {
      this.task = null
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
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

.slide-fade-enter {
  transform: translateX(30px);
  opacity: 0;
}

.slide-fade-enter-active {
  transition: all 0.2s ease;
}

.slide-fade-leave-active {
  transition: all 0.2s ease;
}

.slide-fade-leave-to {
  transform: translateX(-30px);
  opacity: 0;
}
</style>
