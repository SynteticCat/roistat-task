<template>
  <div id="app">
    <div class="content__wrapper">
      <Users/>
      <!-- <About class="about"/> -->
    </div>
  </div>
</template>

<script>
import Users from './components/Users/Users';
import About from './components/About';

export default {
  name: 'App',
  components: { Users, About }
}
</script>

<style scoped>
  .content__wrapper {
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .about {
    max-width: 400px;
    margin-left: 50px;
  }
</style>
