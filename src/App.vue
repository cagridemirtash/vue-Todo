<template>
  <Header />
  <hr>
  <Todo />
  <hr>
  <Footer class="fixed-bottom"/>
</template>

<script>
import Header from './components/Header'
import Todo from './components/Todo'
import Footer from './components/Footer'

export default {
  name: 'App',
  components: {
    Todo,
    Footer,
    Header
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
}
</style>
