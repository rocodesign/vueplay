<template>
  <ul class="rdx-list" :class="listclass">
    <li v-for="(item, i) in items"
      v-bind:key="item.id"
      @click="onClickItem(item)"
      :class="{'selected': selectedIndex === i}">
        <span v-if="renderer">
          <component :is="renderer" v-bind="item"/>
        </span>
        <span v-else>{{ computeLabel(item) }}</span>
    </li>
  </ul>
</template>

<script>
export default {
  name: 'Autocomplete',
  props: {
    items: { default: () => [] },
    listclass: { type: String, default: '' },
    labelField: { type: String, default: 'label' },
    labelFunction: { type: Function,
      default: null },
    selectedIndex: { type: Number, default: -1 },
    renderer: null,
  },
  methods: {
    computeLabel(item) {
      if (this.labelFunction) {
        return this.labelFunction(item);
      }
      return item[this.labelField];
    },
    onClickItem(item) {
      this.$emit('selected', item);
    },
  },
  computed: {},
  data() {
    return {};
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="postcss" scoped>
ul {
  @apply .px-2 .py-1 .mt-1 .rounded .shadow-lg;
  list-style-type: none;
  background: white;
  display: block;
  border: 2px solid #5433be;

  li {
    @apply .p-2;
    border-bottom: 2px solid #5433be;
    background: white;
    color: black;
    text-align: left;

    &:last-child {
      border-bottom: 0;
    }

    &.selected {
      @apply .font-medium .bg-purple-lighter;
    }

    &:hover {
      @apply .bg-purple-lightest;
    }
  }
}
</style>
