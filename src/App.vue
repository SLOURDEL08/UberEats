<template>

    <router-view></router-view>
</template>

<script>
import NavBar from './components/NavBar.vue';
export default {
  name: 'App',
  components: NavBar
};
</script>

<style>
@import url('./style.css');
@import url('https://fonts.googleapis.com/css2?family=Roboto%3Awght%40300%3B400%3B700&display=swap');

#app {
  font-family: 'Roboto', sans-serif;
  padding: 0px 20px;
}
</style>
