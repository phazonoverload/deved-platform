<template>
    <div>
        <h2>Chapter</h2>
        <pre>{{chapter}}</pre>
    </div>
</template>

<script>
export default {
  async asyncData({ $content, params, error }) {
    try {
      const chapter = await $content(`courses/classes/chapters/${params.chapter}`)
        .fetch()
        .catch(err => error({ statusCode: 404, message: 'Page not found', err }))

      return { chapter }
    } catch (e) {
      error(e)
      return false
    }
  }
}
</script>

<style>

</style>
