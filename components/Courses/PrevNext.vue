<template>
  <div class="links" v-if="prev || next">
    <div class="prev">
      <NuxtLink v-if="prev" :to="prev.slug" class="Vlt-btn Vlt-btn--small Vlt-btn--primary">
        <svg><use xlink:href="../../node_modules/@vonagevolta/volta2/dist/symbol/volta-icons.svg#Vlt-icon-arrow-left-full"/></svg>{{ prev.title }}
      </NuxtLink>
      <span v-else>{{start}}</span>
    </div>
    <div class="next">
      <NuxtLink v-if="next" :to="next.slug" class="Vlt-btn Vlt-btn--small Vlt-btn--primary">
        {{ next.title }}<svg><use xlink:href="../../node_modules/@vonagevolta/volta2/dist/symbol/volta-icons.svg#Vlt-icon-arrow-right-full"/></svg>
      </NuxtLink>
      <span v-else>{{end}}</span>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    prev: { type: Object, default: () => null },
    next: { type: Object, default: () => null },
    start: { type: String, default: () => ' ' },
    end: { type: String, default: () => ' ' },
  }
}
</script>

<style scoped>
.links {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.next .Vlt-btn svg {
  margin-right: 0;
  margin-left: 8px;
}
@media screen and (max-width: 775px) {
  .links {
    flex-direction: column;
  }
  .prev {
    display: none;
  }
}
</style>
