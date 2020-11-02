<template>
    <div>
        <h2>Class</h2>
        <pre>{{singleClass}}</pre>
        <h2>Classes</h2>
        <ul>
          <li v-for="chapter in chapters" :key="chapter.slug">
            <pre>{{chapter}}</pre>
          </li>
        </ul>
    </div>
</template>

<script>
export default {
  async asyncData({ $content, params, error }) {
    try {
      const singleClass = await $content(`courses/classes/${params.class}`)
        .fetch()
        .catch(err => error({ statusCode: 404, message: 'Page not found', err }))
        console.log(singleClass)

      const chapters = await $content(`courses/classes/chapters`)
        .where({ class: { $eq: params.class } })
        .sortBy('order', 'asc')
        .fetch()

      return { singleClass, chapters }
    } catch (e) {
      error(e)
      return false
    }
  }
}
</script>

<style>

</style>
