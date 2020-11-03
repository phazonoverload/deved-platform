<template>
  <div>
    <img class='header' :src='course.header' :alt='course.title'>
    <div class='Vlt-container'>
      <div class="Vlt-card__header Vlt-margin--A-top3 Vlt-center">
        <h1 property="headline">
          {{ course.title }}
        </h1>
        <BackToTop />
      </div>
    </div>
    <!-- <pre>{{course}}</pre> -->
    <!-- <h2>Classes</h2>
    <ul>
      <li v-for="c in classes" :key="c.slug">
        <pre>{{c}}</pre>
      </li>
    </ul> -->
  </div>
</template>

<script>
export default {
  async asyncData({ $content, params, error }) {
    try {
      const course = await $content(`courses/${params.course}`)
        .fetch()
        .catch(err => error({ statusCode: 404, message: 'Page not found', err }))

      const classes = await $content(`courses/classes`)
        .where({ course: { $eq: params.course } })
        .sortBy('order', 'asc')
        .fetch()

      return { course, classes }
    } catch (e) {
      error(e)
      return false
    }
  }
}
</script>

<style>

img.header {
  width: 100%;
  margin-top: -1em;
}
</style>
