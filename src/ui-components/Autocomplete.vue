<template>
  <div class="rdx-autocomplete">
    <input type="search" v-model="text" @input="onChange" />
    <List v-bind:items="filteredItems"
      v-bind:label-field="labelField"
      v-bind:label-function="labelFunction"/>
  </div>
</template>

<script>
import List from './List';

export default {
  name: 'Autocomplete',
  components: {
    List,
  },
  props: {
    items: { default: () => [] },
    labelField: { type: String, default: 'label' },
    labelFunction: { type: Function, default: null },
    filterFunction: { type: Function, default: null },
  },
  methods: {
    onChange() {
      this.filter(this.text.toLowerCase());
      this.$emit('change');
    },
    filter(text) {
      if (text) {
        const filterFunction = this.filterFunction || this.itemFilter;
        this.filteredItems = this.items.filter(item => filterFunction(item, text));
      } else {
        this.filteredItems = this.items;
      }
    },
    itemFilter(item, text) {
      return !!Object.values(item).find(val =>
        typeof val === 'string' &&
        val.toLowerCase().indexOf(text) >= 0);
    },
  },
  data() {
    return {
      filteredItems: this.items || [],
      isOpen: false,
      text: '',
    };
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.rdx-autocomplete {
  display: block;
}

input {
  outline: none;
  border-radius: 0.25rem;
  border-style: solid;
  border-color: #5433be;
  border-width: 1px;
  padding: 0.5rem;
  width: 100%;
}
</style>
