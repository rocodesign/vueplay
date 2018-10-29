<template>
  <div class="rdx-autocomplete" :class="{dirty, valid, invalid: dirty && !valid}">
    <input type="search" v-model="text" @input="onChange" @focus="onFocus"
      @click="onFocus" @blur="onBlur"
      @keydown.up="onKeyUp" @keydown.down="onKeyDown" @keydown.enter="onKeyEnter"/>
    <List
      ref="list"
      v-bind:items="filteredItems"
      v-bind:label-field="labelField"
      v-bind:label-function="labelFunction"
      v-bind:listclass="listClass"
      v-bind:selected-index="selectedIndex"
      v-bind:renderer="renderer"
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
    validator: { type: Function, default: data => !!data },
    renderer: null,
  },
  methods: {
    onChange() {
      this.searchText = this.text;
      this.listOpen = true;
      this.dirty = true;
      this.selectedItem = null;
      this.$emit('change', this.text);
    },
    onFocus() {
      this.searchText = '';
      this.listOpen = true;
    },
    onBlur() {
      setTimeout(this.closeList, 100);
    },
    onKeyUp() {
      this.selectedIndex = ((this.selectedIndex - 1) + this.itemNo) % this.itemNo;
      this.scrollList();
    },
    onKeyDown() {
      this.selectedIndex = (this.selectedIndex + 1) % this.itemNo;
      this.scrollList();
    },
    scrollList() {
      const listel = this.$refs.list.$el;
      const scrollPercent = this.selectedIndex / (this.itemNo - 1);
      const scrollMax = listel.scrollHeight - listel.clientHeight;
      const visibleMin = listel.scrollTop / listel.scrollHeight;
      const visibleMax = (listel.clientHeight + listel.scrollTop) / listel.scrollHeight;
      if (scrollPercent > visibleMax || scrollPercent < visibleMin) {
        listel.scrollTop = scrollMax * scrollPercent;
      }
    },
    onKeyEnter() {
      if (!this.listOpen) {
        this.listOpen = true;
        this.searchText = '';
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
        this.dirty = true;
        this.selectedItem = item;
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
      return this.listOpen && this.itemNo ? 'open' : '';
    },
    valid() {
      return this.dirty && this.validator(this.selectedItem || this.text);
    },
  },
  watch: {
    valid(newValue, oldValue) {
      if (newValue !== oldValue) {
        this.$emit('valid-change', newValue);
      }
    },
  },
  data() {
    return {
      isOpen: false,
      dirty: false,
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
    width: 100%;
  }

  &.invalid {
    input {
      @apply .border-red;
    }
  }

  .rdx-list {
    @apply .absolute .pin-l .pin-r .z-50;

    max-height: 0;
    overflow-x: hidden;
    overflow-y: scroll;
    opacity: 0;
    transition: 300ms all;
    &.open {
      max-height: 100vh;
      opacity: 1;
    }
  }
}
</style>
