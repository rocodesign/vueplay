<template>
  <div id="app">
    <img src="./assets/logo.png">
    <RecipientSelector />
  </div>
</template>

<script>
import RecipientSelector from './components/RecipientSelector';
// import Autocomplete from './ui-components/Autocomplete';
import List from './ui-components/List';

export default {
  name: 'App1',
  components: {
    RecipientSelector,
    List,
  },

};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.pr-1 {
  padding-top: 1rem;
}
</style>

<style lang="postcss">
@import "./styles.css";
</style>

