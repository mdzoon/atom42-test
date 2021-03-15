<template>
  <li v-if="entry">
    Your {{ name }}: {{ entry }}
  </li>
</template>

<script>
export default {
  props: { 
    entry: String,
    name: String
  },
}
</script>


<style>
  li {
    list-style-type: none;
  }
</style>
