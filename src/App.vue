<script setup>
import Main from './components/Main.vue';
</script>
<template>
  <div class="flex justify-center container mt-20">
    <Main />
  </div>
</template>
<style lang="scss">
body {
  background-color: #020617;
}


@media only screen and (max-width: 600px) {
  .container {
    margin-top: 20px;
  }
}
</style>