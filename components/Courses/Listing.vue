<template>
  <ol>
    <li v-for="(link, i) in links" :key="i">
      <a :href="link.url">
        <span><b>{{link.text.main}}</b></span>
        <span v-if="link.text.secondary">{{link.text.secondary}}</span>
      </a>
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

<style scoped>
ol {
  border: 1px solid lightgrey;
  border-radius: 6px;
}
li + li {
  border-top: 1px solid lightgrey;
}
li a {
  display: block;
  padding: 1em;
  color: #131415 !important;
  text-decoration: none !important;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}
</style>
