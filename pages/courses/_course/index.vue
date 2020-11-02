<template>
    <div>
        {{course.name}}
    </div>
</template>

<script>
export default {
  async asyncData({ $content, params, error }) {
    try {
      const course = await $content(`courses/${params.course}`)
        .fetch()
        .catch((err) => {
          console.log('err', err)
          error({ statusCode: 404, message: 'Page not found', err })
        })
      return { course }
    } catch (e) {
      error(e)
      return false
    }
  }
}
</script>

<style>

</style>
