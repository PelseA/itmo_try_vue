<template>
  <p class="value">
    <span style="color: blue;">{{ title }}</span>: {{ value }}
  </p>
</template>

<script>
export default {
  props: {
    title: '',
    value: 0,
  },
};
</script>

<!-- scoped - css будет применен только на этой странице -->
<style scoped>
p.value {
  color: red;
  font-size: 2rem;
  font-weight: bold;
}
</style>