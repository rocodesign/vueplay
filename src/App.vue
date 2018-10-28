<template>
  <div id="app">
    <img src="./assets/logo.png">
    <ContactSelector />
  </div>
</template>

<script>
import ContactSelector from './components/ContactSelector';

export default {
  name: 'App1',
  components: {
    ContactSelector,
  },

};
</script>

<style>
#app {
  font-family: "Roboto", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>

<style lang="postcss">
@import "./styles.css";

#app {
  @apply .flex .flex-col .items-center .justify-around .w-full .h-full .p-16 .pt-32;
}
</style>

