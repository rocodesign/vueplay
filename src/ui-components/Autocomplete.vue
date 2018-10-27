<template>
  <div class="rdx-autocomplete">
    <input type="search" v-model="text" @input="onChange" @focus="onFocus" @blur="onBlur"
      @keydown.up="onKeyUp" @keydown.down="onKeyDown" @keydown.enter="onKeyEnter"/>
    <List v-bind:items="filteredItems"
      v-bind:label-field="labelField"
      v-bind:label-function="labelFunction"
      v-bind:listclass="listClass"
      v-bind:selected-index="selectedIndex"
      @selected="onItemSelected"/>
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
      this.searchText = this.text;
      this.$emit('change', this.text);
    },
    onFocus() {
      this.searchText = '';
      this.listOpen = true;
    },
    onBlur() {
      this.closeList();
    },
    onKeyUp() {
      this.selectedIndex = ((this.selectedIndex - 1) + this.itemNo) % this.itemNo;
    },
    onKeyDown() {
      this.selectedIndex = (this.selectedIndex + 1) % this.itemNo;
    },
    onKeyEnter() {
      if (!this.listOpen) {
        this.listOpen = true;
        return;
      }
      let item;
      if (this.selectedIndex >= 0) {
        item = this.filteredItems[this.selectedIndex];
      } else if (this.itemNo === 1) {
        item = this.filteredItems[0];
      }
      this.onItemSelected(item);
    },
    onItemSelected(item) {
      if (item) {
        this.text = this.computeLabel(item);
        this.$emit('selected', item);
      }
      this.listOpen = false;
    },
    closeList() {
      this.selectedIndex = -1;
      this.listOpen = false;
    },
    itemFilter(item, text) {
      return !!Object.values(item).find(val =>
        typeof val === 'string' &&
        val.toLowerCase().indexOf(text) >= 0);
    },
    computeLabel(item) {
      if (this.labelFunction) {
        return this.labelFunction(item);
      }
      return item[this.labelField];
    },
  },
  computed: {
    filteredItems() {
      if (this.searchText) {
        const text = this.searchText.toLowerCase();
        const filterFunction = this.filterFunction || this.itemFilter;
        return this.items.filter(item => filterFunction(item, text));
      }
      return this.items;
    },
    itemNo() {
      return this.filteredItems.length;
    },
    listClass() {
      return this.listOpen ? 'open' : '';
    },
  },
  data() {
    return {
      isOpen: false,
      text: '',
      searchText: '',
      listOpen: false,
      selectedIndex: -1,
      selectedItem: null,
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
