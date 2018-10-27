<template>
  <ul class="rdx-list">
    <li v-for="item in items"
      v-bind:key="item.id">
        {{ computeLabel(item) }}
    </li>
  </ul>
</template>

<script>
export default {
  name: 'Autocomplete',
  props: {
    items: { default: () => [] },
    labelField: { type: String, default: 'label' },
    labelFunction: { type: Function,
      default: null },
  },
  methods: {
    computeLabel(item) {
      if (this.labelFunction) {
        return this.labelFunction(item);
      }
      return item[this.labelField];
    },
  },
  computed: {},
  data() {
    return {
      something: '',

    };
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="postcss" scoped>
ul {
  @apply .px-2 .pt-2 .-mt-2;
  list-style-type: none;
  background: white;
  display: block;
  border: 1px solid #5433be;
  border-top: 0;

  li {
    @apply .p-2;
    border-bottom: 1px solid #5433be;
    background: white;
    color: black;
    text-align: left;

    &:last-child {
      border-bottom: 0;
    }
  }
}
</style>
