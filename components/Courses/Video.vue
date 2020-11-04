<template>
  <div class="video">
      <iframe :src="`https://www.youtube.com/embed/${id}`" width="560" height="349" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
  </div>
</template>

<script>
export default {
  props: {
    id: {
      type: String,
      required: true,
    },
  },
}
</script>

<style scoped>
.video {
  position: relative;
  padding-bottom: 56.25%; /* 16:9 */
  height: 0;
	overflow: hidden;
	border-radius: 6px;
}
iframe {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
</style>
