<template>
  <div id="app">
    <div class="container">
      <div class="d-flex-m flex-space-between flex-align-items-center p20">
        <img src="./assets/logo.png" class="img-small">
        <h1 class="title-app">List of tasks</h1>
      </div>
      <list-tasks></list-tasks>
    </div>
  </div>
</template>

<script>
import ListTasks from './components/ListTasks'

export default {
  name: 'App',
  components: {
    ListTasks
  }
}
</script>

<style>
*{
  /* margin: 0;
  border: 0;
  padding: 0; */
  box-sizing: border-box;
}

:root{
  --color-inputs: #2c3e50;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: var(--color-inputs);
  margin-top: 60px;
}
.container{
  width: 60%;
  margin: 0 auto;
}
.img-small{
  display:block;
  width: 90px;
  flex-grow: 1;
}
.title-app{
  flex-grow: 10;
  text-align: center;
}

@media(max-width: 759px) {
    
}
</style>
