<template>
  <div id="app">
    <Employee />
  </div>
</template>

<script>
import Employee from './components/Employee.vue';

export default {
  name: 'App',
  components: {
    Employee
  }
};
</script>

<style>
body {
  background-color: #f8f9fa;
}
</style>
