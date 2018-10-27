<template>
  <div class="rdx-autocomplete">
    <input type="search" v-model="text" @input="onChange" @focus="onFocus" @blur="onBlur"
      @keydown.up="onKeyUp" @keydown.down="onKeyDown"/>
    <List v-bind:items="filteredItems"
      v-bind:label-field="labelField"
      v-bind:label-function="labelFunction"
      v-bind:listclass="listClass"
      v-bind:selected-index="selectedIndex"/>
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
      this.$emit('change');
    },
    onFocus() {
      this.listClass = 'open';
    },
    onBlur() {
      this.listClass = '';
    },
    onKeyUp() {
      this.selectedIndex = ((this.selectedIndex - 1) + this.itemNo) % this.itemNo;
    },
    onKeyDown() {
      this.selectedIndex = (this.selectedIndex + 1) % this.itemNo;
    },

    itemFilter(item, text) {
      return !!Object.values(item).find(val =>
        typeof val === 'string' &&
        val.toLowerCase().indexOf(text) >= 0);
    },
  },
  computed: {
    filteredItems() {
      if (this.text) {
        const text = this.text.toLowerCase();
        const filterFunction = this.filterFunction || this.itemFilter;
        return this.items.filter(item => filterFunction(item, text));
      }
      return this.items;
    },
    itemNo() {
      return this.filteredItems.length;
    },
  },
  data() {
    return {
      isOpen: false,
      text: '',
      listClass: '',
      selectedIndex: -1,
    };
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="postcss">
.rdx-autocomplete {
  @apply .relative;

  display: block;

  input {
    outline: none;
    border-radius: 0.25rem;
    border-style: solid;
    border-color: #5433be;
    border-width: 2px;
    padding: 0.5rem;
    width: 100%;
  }

  .rdx-list {
    @apply .absolute .pin-l .pin-r;

    max-height: 0;
    overflow: hidden;
    opacity: 0;
    transition: 300ms all;
    &.open {
      max-height: 100vh;
      opacity: 1;
    }
  }
}
</style>
