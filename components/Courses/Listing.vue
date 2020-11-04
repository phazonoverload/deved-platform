<template>
  <ol>
    <li v-for="(link, i) in links" :key="i">
      <a :href="link.path">{{link.text}}</a>
    </li>
  </ol>
</template>

<script>
export default {
  props: {
    links: {
      type: Array,
      required: true,
    },
  },
}
</script>

<style>

</style>
