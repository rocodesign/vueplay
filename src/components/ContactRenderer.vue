<template>
  <div>
    <div class="mb-1">{{ alias || '---'}} </div>
    <div class="text-sm text-grey-darker">{{address}} </div>
  </div>
</template>

<script>
export default {
  name: 'ContactRenderer',
  props: {
    alias: '',
    address: '',
  },
};
</script>

