<script>
export default {
  props: {
    msg: String
  },
  emits: ['response'],
  created() {
    this.$emit('response', 'hello from child')
  }
}
</script>

<template>
  <h2>{{ msg || 'No props passed yet' }}</h2>
  <h2>Child component</h2>
</template>