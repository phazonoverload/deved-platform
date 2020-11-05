<template>
  <div class="Vlt-container">
      <div class="header">
        <h1>Courses</h1>
      </div>
      <nuxt-link :to="`/courses/${course.slug}`" class="Vlt-card Vlt-bg-white" v-for="course in courses" :key="course.slug">
        <img :src="course.thumbnail" :alt="course.title">
        <div class="info">
          <h1 property="headline">
            {{ course.title }}
          </h1>
          <h2 class='summary'>{{course.summary}}</h2>
        </div>
      </nuxt-link>
  </div>
</template>

<script>
export default {
  async asyncData({ $content, params, error }) {
    try {
      let courses = await $content('courses').fetch()
      return { courses }
    } catch (e) {
      error(e)
      return false
    }

    function courseLength(times) {
      const timestamp = times.reduce((acc, item) => acc + item, 0)
      const h = Math.floor(timestamp / 60 / 60)
      const m = Math.floor(timestamp / 60) - (h * 60)
      return h.toString().padStart(2, '0') + ':' + m.toString().padStart(2, '0')
    }
  }
}
</script>

<style scoped>
.header {
  text-align: center;
  padding: 2em 0;
}
img {
  border-radius: 6px;
  margin-bottom: 1em;
}
h1 {
  margin-bottom: 1rem;
}
h2 {
  margin-top: 0;
  margin-bottom: 0;
}
</style>
