<template>
  <div id="app">
    <img src="./assets/logo.png">
    <RecipientSelector />
  </div>
</template>

<script>
import RecipientSelector from './components/RecipientSelector';

export default {
  name: 'App1',
  components: {
    RecipientSelector,
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
  @apply .w-full .h-full .p-16 .pt-32;
}
</style>

